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
  font-size: 6rem;
  font-weight: 600;
  margin-bottom: -3.35rem;
  opacity: 0.1;
  padding: 0 2rem;
}
</style>
