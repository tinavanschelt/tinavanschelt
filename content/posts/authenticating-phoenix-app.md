---
title: Authenticating your Phoenix 1.5 app with Guardian, Argon2 and Comeonin
date: 2020-05-10
published: false
tags: ['elixir', 'phoenix']
series: false
canonical_url: false
description: ''
---

quick intro

#### Information that might be useful upfront

If you follow along, you should end up with something similar to [ heroku link ].

Phoenix 1.5 just got released, you can find the upgrade instructions [here](https://gist.github.com/chrismccord/e53e79ef8b34adf5d8122a47db44d22f).

If you don’t already have Phoenix, Elixir and PostgreSQL good-to-go on your system, [start here](https://hexdocs.pm/phoenix/installation.html#content).

You can find the more official [“Getting Started with Guardian”](https://hexdocs.pm/phoenix/installation.html#content) in the Guardian docs.

#### An overview of the tools we'll be using

** [Guardian](https://hexdocs.pm/guardian/introduction-overview.html#content) **

An authentication toolkit for Elixir with two main features: **creating tokens** and **verifying tokens**.

** [Argon2](https://en.wikipedia.org/wiki/Argon2) **

Argon 2 (the successor to Bcrypt) is a multi-year project that won the [Password Hashing Competition](https://password-hashing.net/). The [official repo](https://github.com/P-H-C/phc-winner-argon2) describes it as

> a password-hashing function that summarizes the state of the art in the design of memory-hard functions and can be used to hash passwords for credential storage, key derivation, or other applications.

** Comeonin **

Comeonin is a specification for password hashing libraries. You'll find the wiki [here](https://github.com/riverrun/comeonin/wiki), where they provide information on alternative password hashing libraries like Bcrypt and Pbkdf2 too.

#### An overview of what we'll be doing

Right, assuming that you now have the Phoenix 1.5 project generator and Elixir >= 1.7 on your system, let’s dive in.

#### Create a new Phoenix application and add the required dependencies

First off, generate your application

```
$ mix phx.new auth_app # Replace auth_app with your application name and if prompted to install dependencies say yes
```

Let’s make sure your app was generated as expected. As per the instructions in your terminal, `cd` into your application folder and run `$ mix ecto.create` to create the PostgreSQL database for your application. In my case, the generated database is called `auth_app_dev`.

Now run `$ mix phx.server`, the Phoenix start page should be showing up when you visit `http://localhost:4000`.

If everything looks as expected, jump to `mix.exs` and add the latest versions of [Guardian](https://github.com/ueberauth/guardian) and [Argon2](https://github.com/riverrun/argon2_elixir) to your dependencies

```
# mix.exs

defp deps do
  [
    ...
    {:guardian, "~> 2.0"},
    {:argon2_elixir, "~> 2.0"},
  ]
end
```

Run `$ mix deps.get` to install your newly added dependencies.

#### Create your User model

We’re going to be creating a `User` model within an `Accounts` context. If you’re unfamiliar with contexts, I would encourage you to read up on it. The Phoenix documentation covers [contexts](https://hexdocs.pm/phoenix/contexts.html) in depth. Our users will be signing up with email and we want to store their first and last names, so our mix task will look as follows:

```
$ mix phx.gen.context Account User users email:string first_name:string last_name:string password:string
```

After running the above command, you should have an updated folder structure:

```
/lib
--/auth_app
----/account
------/user.ex
----/account.ex
```

Your `account.ex` file should be prepopulated with the following methods (I removed the @doc comments for the sake of brevity):

```
defmodule AuthApp.Account do
  import Ecto.Query, warn: false

  alias AuthApp.Repo
  alias AuthApp.Account.User

  def list_users do
    Repo.all(User)
  end

  def get_user!(id), do: Repo.get!(User, id)

  def create_user(attrs \\ %{}) do
    %User{}
    |> User.changeset(attrs)
    |> Repo.insert()
  end

  def update_user(%User{} = user, attrs) do
    user
    |> User.changeset(attrs)
    |> Repo.update()
  end

  def delete_user(%User{} = user) do
    Repo.delete(user)
  end

  def change_user(%User{} = user, attrs \\ %{}) do
    User.changeset(user, attrs)
  end
end
```

Lastly, run `mix ecto.migrate`to update your database with the generated schema. If the migration was successful your `auth_app_dev` database should now have a `users` table.

#### Create an “implementation module” for Guardian

I grabbed to standard implementation module from Guardian’s README. The implementation module is an important part of the Guardian ecosystem and is well covered in their [documentation](https://hexdocs.pm/guardian/introduction-implementation.html#content). In short, the implementation module

```
# lib/auth_app/account/guardian.ex

defmodule AuthApp.Account.Guardian do
  use Guardian, otp_app: :auth_app

  def subject_for_token(resource, _claims) do
    # You can use any value for the subject of your token but
    # it should be useful in retrieving the resource later, see
    # how it being used on `resource_from_claims/1` function.
    # A unique `id` is a good subject, a non-unique email address
    # is a poor subject.
    sub = to_string(resource.id)
    {:ok, sub}
  end

  def subject_for_token(_, _) do
    {:error, :reason_for_error}
  end

  def resource_from_claims(claims) do
    # Here we'll look up our resource from the claims, the subject can be
    # found in the `"sub"` key. In `above subject_for_token/2` we returned
    # the resource id so here we'll rely on that to look it up.
    id = claims["sub"]
    resource = AuthApp.get_resource_by_id(id)
    {:ok,  resource}
  end

  def resource_from_claims(_claims) do
    {:error, :reason_for_error}
  end
end
```

#### Setup environment variables and configure Guardian

You will require a secret key for you Guardian configuration and the best place to store said key is in your app’s environment variables. It’s better to do this from the get go so that you don’t accidentally commit sensitive information to GitHub.

You can follow this guide to setup the environment variable.

Run `$ mix guardian.gen.secret` to generate your secret key, add it to `.env` and then add the following configuration to your `config/config.exs` file:

```
# config/config.exs

# Guardian configuration
config :auth_app, AuthApp.Account.Guardian,
  issuer: "auth_app",
  secret_key: System.get_env("SECRET_KEY")
```

I named my variable “SECRET_KEY”, be sure to replace it if you opted for a different name. If you don’t feel like setting up environment variables now, you can always just replace “SECRET_KEY” with the string generated by the `mix guardian.gen.secret` command.

#### Password hashing and user validation

As mentioned, we’re using Argon2 for password hashing. We’ve already added the dependency, now we want to utilise this when a user signs up or logs in.

Open up `user.ex`, it should look like this:

```
# lib/auth_app/account/user.ex

defmodule AuthApp.Account.User do
  use Ecto.Schema
  import Ecto.Changeset

  schema "users" do
    field :email, :string
    field :first_name, :string
    field :last_name, :string
    field :password, :string
    timestamps()
  end

  @doc false
  def changeset(user, attrs) do
    user
    |> cast(attrs, [:email, :first_name, :last_name, :password])
    |> validate_required([:email, :first_name, :last_name, :password])
  end
end
```

First, our login and sign up form will only have an email and a password field, so we want to remove :first_name and :last_name from `validate_required` and update the user changeset to include validation for the email and password fields.

```
# lib/auth_app/account/user.ex

defmodule AuthApp.Account.User do
  ...
  @doc false
  def changeset(user, attrs) do
    user
    |> cast(attrs, [:email, :first_name, :last_name, :password])
    |> validate_required([:email, :password])
    |> validate_format(:email, ~r/@/)
    |> validate_length(:password, min: 8)
    |> put_password_hash()
  end
  ...
end
```

Lastly, we want to add a `put_password_hash` method to hash the user's text password. Your complete `user.ex` file should look as follows:

```
# lib/auth_app/account/user.ex

defmodule AuthApp.Account.User do
  use Ecto.Schema
  import Ecto.Changeset
  alias Argon2

  schema "users" do
    field :email, :string
    field :first_name, :string
    field :last_name, :string
    field :password, :string

    timestamps()
  end

  @doc false
  def changeset(user, attrs) do
    user
    |> cast(attrs, [:email, :first_name, :last_name, :password])
    |> validate_required([:email, :password])
    |> validate_format(:email, ~r/@/)
    |> validate_length(:password, min: 8)
    |> put_password_hash()
  end

  defp put_password_hash(%Ecto.Changeset{valid?: true, changes: %{password: password}} = changeset) do
    change(changeset, password: Argon2.hash_pwd_salt(password))
  end

  defp put_password_hash(changeset), do: changeset
end
```

#### Authentication

We still need to authenticate our users. I’m using the same authentication method and pipelines as used in the “Getting Started with Guardian” guide. Add `authenticate_user` to your Account context (make sure you add it _after_ `alias AuthApp.Account.User`)

---

```
# lib/authapp/account.ex

defmodule AuthApp.Account do
  ...
  import Ecto.Query, warn: false
  alias AuthApp.Repo
  alias AuthApp.Account.User
  alias Argon2

  def authenticate_user(email, plain_text_password) do
    query = from u in User, where: u.email == ^email
    case Repo.one(query) do
      nil ->
        Argon2.no_user_verify()
        {:error, :invalid_credentials}
      user ->
        if Argon2.verify_pass(plain_text_password, user.password) do
          {:ok, user}
        else
          {:error, :invalid_credentials}
        end
    end
  end
  ...
end
```

Now we need to setup our Pipelines to funnel our user sessions and header authentication.

```
# lib/auth_app/account/pipeline.ex

defmodule AuthApp.Account.Pipeline do
  use Guardian.Plug.Pipeline,
    otp_app: :auth_app,
    error_handler: AuthApp.Account.ErrorHandler,
    module: AuthApp.Account.Guardian

    # If there is a session token, restrict it to an access token and validate it
    plug Guardian.Plug.VerifySession, claims: %{"typ" => "access"}
    # If there is an authorization header, restrict it to an access token and validate it
    plug Guardian.Plug.VerifyHeader, claims: %{"typ" => "access"}
    # Load the user if either of the verifications worked
    plug Guardian.Plug.LoadResource, allow_blank: true
end
```

Error handler

```
# lib/auth_app/account/error_handler.ex

defmodule AuthApp.Account.ErrorHandler do
  import Plug.Conn
  @behaviour Guardian.Plug.ErrorHandler
  @impl Guardian.Plug.ErrorHandler
  def auth_error(conn, {type, _reason}, _opts) do
    body = to_string(type)
    conn
    |> put_resp_content_type("text/plain")
    |> send_resp(401, body)
  end
end
```

#### Configure User Pipelines and Sessions

When a user logs in or out we need to manage that session, so we’re going to add a Session Controller that utilises the `authenticate_user` method, along with session views and templates.

Create your session controller:

```
# lib/auth_app_web/controllers/session_controller.ex

defmodule AuthAppWeb.SessionController do
  use AuthAppWeb, :controller
  alias AuthApp.{Account, Account.User, Account.Guardian}

  def new(conn, _) do
    changeset = Account.change_user(%User{})
    maybe_user = Guardian.Plug.current_resource(conn)
    if maybe_user do
      redirect(conn, to: "/protected")
    else
      render(conn, "new.html", changeset: changeset, action: Routes.session_path(conn, :login))
    end
  end

  def login(conn, %{"user" => %{"email" => email, "password" => password}}) do
    Account.authenticate_user(email, password)
    |> login_reply(conn)
  end

  def logout(conn, _) do
    conn
    |> Guardian.Plug.sign_out() #This module's full name is Auth.Account.Guardian.Plug,
    |> redirect(to: "/login")   #and the arguments specfied in the Guardian.Plug.sign_out()
  end                           #docs are not applicable here

  defp login_reply({:ok, user}, conn) do
    conn
    |> put_flash(:info, "Welcome back!")
    |> Guardian.Plug.sign_in(user)   #This module's full name is Auth.Account.Guardian.Plug,
    |> redirect(to: "/protected")    #and the arguments specified in the Guardian.Plug.sign_in()
  end                                #docs are not applicable here.

  defp login_reply({:error, reason}, conn) do
    conn
    |> put_flash(:error, to_string(reason))
    |> new(%{})
  end
end
```

Your session view:

```
# lib/auth_app_web/views/session_view.ex

defmodule AuthAppWeb.SessionView do
  use AuthAppWeb, :view
end
```

Your login template:

```
# lib/auth_app_web/templates/session/new.html.eex

<h2>Login Page</h2>

<%= form_for @changeset, @action, fn f -> %>
  <div class="form-group">
    <%= label f, :email, class: "control-label" %>
    <%= text_input f, :email, class: "form-control" %>
    <%= error_tag f, :email %>
  </div>

  <div class="form-group">
    <%= label f, :password, class: "control-label" %>
    <%= password_input f, :password, class: "form-control" %>
    <%= error_tag f, :password %>
  </div>

  <div class="form-group">
    <%= submit "Submit", class: "btn btn-primary" %>
  </div>
<% end %>
```

Add the routes for your session to `auth_app_web/router.ex`

```
...
  pipeline :auth do
    plug AuthApp.Account.Pipeline
  end

  pipeline :ensure_auth do
    plug Guardian.Plug.EnsureAuthenticated
  end

  pipeline :api do
    plug :accepts, ["json"]
  end

  scope "/", AuthAppWeb do
    pipe_through [:browser, :auth]

    get "/", PageController, :index

    get "/login", SessionController, :new
    post "/login", SessionController, :login
    get "/logout", SessionController, :logout
  end
...
```
