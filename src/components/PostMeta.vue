<template>
  <div class="post-meta">
    <span>Posted {{ post.date }}</span>
    <template v-if="post.timeToRead">
      <span class="time-to-read">
        <strong>{{ post.timeToRead }} min read</strong>
      </span>
    </template>
  </div>
</template>

<script>
export default {
  props: ["post"]
};
</script>

<style lang="scss">
.post-meta {
  display: flex;
  flex-direction: column;
  font-size: 0.8em;
  opacity: 0.8;

  .time-to-read {
    color: var(--link-color);
  }
}
</style>
