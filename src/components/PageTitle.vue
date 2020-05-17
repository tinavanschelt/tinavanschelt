<template>
  <div class="page-title" v-html="title" />
</template>

<script>
export default {
  props: ["title"]
};
</script>

<style lang="scss">
.page-title {
  color: var(--page-title-color);
  font-size: 5rem;
  font-weight: 600;
  margin-bottom: -2.75rem;
  padding: 0 1rem;

  @media screen and (min-width: 600px) {
    font-size: 6rem;
    margin-bottom: -3.35rem;
    padding: 0 2rem;
  }
}
</style>
