<template>
  <button class="nav-link" :class="{ active: currentView === 'posts' }" @click="changeView('posts')">Posts</button>
</template>

<script>
export default {
  props: ['currentView'],
  methods: {
    changeView(view) {
      this.$emit('change-view', view);
    }
  }
};
</script>

<style scoped>
/* CSS khusus untuk komponen PostMenu */
</style>
