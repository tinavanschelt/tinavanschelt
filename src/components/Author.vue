<template>
  <div class="author">
    <a href="/">
      <g-image
        alt="Author image"
        class="author__image"
        src="~/assets/images/author.jpg"
        width="180"
        height="180"
        blur="5"
      />
    </a>

    <h1 v-if="showTitle" class="author__site-title">Hello, hi 👋🏼</h1>

    <p>
      I'm Tina, a product-focused software developer based in
      <Strikethrough text="Cape Town" />
      <Strikethrough text="Ireland" />The Netherlands.
    </p>

    <div v-if="showBio">
      <p>
        I love building
        <a href="http://www.github.com/tinavanschelt" target="_blank">things</a>
        and, partly due to a keen interest in people and process, I
        always strive to surround myself with
        <a
          href="http://www.weareleaf.com"
          target="_blank"
        >people who are smarter than me</a>.
      </p>

      <p>
        The purpose of this site is to stash and share some of the
        things I learn along the way, exercise my
        <a
          href="/posts"
        >writing</a> muscle and keep track of my
        <a href="/about">personal and professional growth</a>.
      </p>
    </div>

    <p class="author__links">
      <a href="/about">About</a>
      <a href="/posts">Writing</a>
      <a href="https://twitter.com/tinavanschelt" target="_blank">Twitter</a>
      <a href="https://github.com/tinavanschelt" target="_blank">GitHub</a>
    </p>
  </div>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>

<script>
import Strikethrough from "~/components/Strikethrough.vue";

export default {
  props: ["showTitle", "showBio"],
  components: {
    Strikethrough
  }
};
</script>

<style lang="scss">
.author {
  margin: 0 auto;
  max-width: var(--content-width);
  padding: var(--space);

  &__image {
    border-radius: 100%;
    width: 90px;
    height: 90px;
    margin-bottom: 2em;
  }

  &__links {
    a {
      margin: 0 1em 0 0;
    }
  }
}
</style>
