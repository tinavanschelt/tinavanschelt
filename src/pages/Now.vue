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
          <li>Living in The Netherlands 🇳🇱🍻🧀</li>
          <li>
            Working at <a href="https://www.cleverfranke.com/" target="_blank">CLEVERºFRANKE</a> 👩🏼‍💻
          </li>
          <li>Wrapping up a BSc (Hons) in Computing 👩🏼‍🎓</li>
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
        <p>
          <ul>
            <li>
              Working my way through
              <a
                href="https://www.oreilly.com/library/view/head-first-design/0596007124/"
                target="_blank"
              >Head First Design Patterns</a>
            </li>
          </ul>
        </p>
        <p>
          Thoughts on
          <a href="/stash/a-learning-culture/">lifelong learning</a>
        </p>
        <h5>Health</h5>
        <ul>
          <li>Getting back to pre-pandemic fitness. Aiming for 3 x strength and 3 x running sessions (min 20 km) every week</li>
        </ul>
        <h4>What I'm looking forward</h4>
        <ul>
          <li>Settling in the Netherlands and experiencing Dutch culture firsthand</li>
          <li>Seeing friends and family again later this year 🤞🏼</li>
        </ul>
        <p>
          <small>
            Last update: 10/01/2020
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
    title: "/now"
  }
};
</script>

<style lang="scss">
</style>





