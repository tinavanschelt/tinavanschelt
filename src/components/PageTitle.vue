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
  font-size: 6rem;
  font-weight: 600;
  margin-bottom: -3.35rem;
  padding: 0 2rem;
}
</style>
