<template>
    <button class="nav-link" :class="{ active: currentView === 'todos' }" @click="changeView('todos')">Todos</button>
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
  /* CSS khusus untuk komponen TodosMenu */
  </style>
  