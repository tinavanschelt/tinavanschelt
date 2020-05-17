<template>
  <Layout :show-logo="false">
    <!-- Author intro -->
    <Author :show-title="true" />

    <!-- List posts -->
    <div class="posts content">
      <PageTitle title="/now" />
      <div class="post-card content-box">
        <blockquote>
          <p>“You don’t get results by focusing on results. You get results by focusing on the actions that produce results.”</p>
        </blockquote>
        <br />
        <ul>
          <li>Living in Limerick 🇮🇪🐑🍻</li>
          <li>
            Working
            <a href="http://weareleaf.com" target="_blank">@weareleaf</a> 👩🏼‍💻🍃
          </li>
          <li>Studying towards a BSc (Hons) in Computing and Information Technologies through the University of Derby</li>
        </ul>
        <h4>What I'm currently focused on</h4>
        <h5>Personal</h5>
        <ul>
          <li>Meditating daily</li>
          <li>Focusing on reading long-form content</li>
        </ul>
        <h5>Professional</h5>
        <ul>
          <li>Writing more</li>
          <li>Revisiting functional programming concepts</li>
        </ul>
        <h5>Learning</h5>
        <p>Studying managed to takeover this part of my life, but I’m still focusing on reading classic (and some not-so-classic) programming books, currently:</p>
        <ul>
          <li>
            Working my way through
            <a
              href="https://www.oreilly.com/library/view/head-first-design/0596007124/"
              target="_blank"
            >Head First Design Patterns</a>
          </li>
        </ul>
        <p>
          Thoughts on
          <a href="/stash/a-learning-culture/">lifelong learning</a>
        </p>
        <h5>Health</h5>
        <ul>
          <li>Getting stronger (again)</li>
          <li>
            <del>Running 5km in 24 minutes</del> ✔️ (23:58)
          </li>
          <li>Running a sub 4 hour marathon</li>
        </ul>
        <h4>What I'm looking forward</h4>
        <ul>
          <li>
            <del>Visiting Cuba and Mexico</del> (+ celebrating 5 years of married life 🍾)
          </li>
          <li>
            <del>
              Training for the Connemara Marathon (April 2020)
              <del></del>
            </del>
          </li>
          <li>Turning 30 🤞🏼</li>
          <li>
            <del>Moving to Amsterdam 🇳🇱</del>
          </li>
        </ul>
        <p>Humbly learning to be less of a compulsive planner (and that the only inevitable is aging?). Thanks, COVID ✌🏼</p>
        <p>
          <small>
            Last update: 19/03/2020
            <br />This page is part of the
            <a href="https://sivers.org/nowff">“The /now page movement”</a>
          </small>
        </p>
      </div>
    </div>
  </Layout>
</template>

<page-query>
query {
  posts: allPost(filter: { published: { eq: true }}) {
    edges {
      node {
        id
        title
        date (format: "D MMMM YYYY")
        timeToRead
        description
        path
        tags {
          id
          title
          path
        }
      }
    }
  }
}
</page-query>

<script>
import Author from "~/components/Author.vue";
import PageTitle from "~/components/PageTitle.vue";
import PostCard from "~/components/PostCard.vue";

export default {
  components: {
    Author,
    PageTitle,
    PostCard
  },
  metaInfo: {
    title: "Now"
  }
};
</script>

<style lang="scss">
.posts {
  &__title {
    font-size: 6rem;
    font-weight: 600;
    margin-bottom: -3.35rem;
    opacity: 0.1;
    padding: 0 2rem;

    &--year {
      color: var(--page-title-color);
      font-size: 3.5rem;
      margin-bottom: -2rem;
    }
  }
}
</style>





