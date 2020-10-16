<template>
  <Layout :show-logo="false">
    <!-- Author intro -->
    <Author :show-title="true" :show-bio="true" />

    <!-- List posts -->
    <div class="posts content">
      <PageTitle title="latest" />
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
import PageTitle from "~/components/PageTitle.vue";
import PostCard from "~/components/PostCard.vue";

export default {
  components: {
    Author,
    PageTitle,
    PostCard
  },
  metaInfo: {
    title: "Hoi 👋🏼"
  }
};
</script>

<style lang="scss">
</style>
