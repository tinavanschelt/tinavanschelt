<template>
  <Layout :show-logo="false">
    <!-- Author intro -->
    <Author :show-title="true" />

    <!-- List posts -->
    <div class="posts content">
      <div class="posts__title">writing</div>
      <PostCard v-for="edge in $page.posts.edges" :key="edge.node.id" :post="edge.node" />
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
import PostCard from "~/components/PostCard.vue";

export default {
  components: {
    Author,
    PostCard
  },
  metaInfo: {
    title: "Posts"
  }
};
</script>

<style lang="scss">
.post-title {
  margin: 0 auto calc(var(--space) / 2);
  max-width: var(--content-width);
  padding: calc(var(--space) / 2) 0 calc(var(--space) / 2);
  text-align: center;
}

.posts {
  &__title {
    font-size: 6rem;
    font-weight: 600;
    margin-bottom: -3.35rem;
    opacity: 10%;
    padding: 0 2rem;
  }
}
</style>
