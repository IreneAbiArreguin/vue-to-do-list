<script setup lang="ts">
import AppHeader from "./components/AppHeader.vue";
import TodoList from "./components/TodoList.vue";
</script>

<template>
  <div class="app-container">
    <AppHeader />
    <TodoList />
  </div>
</template>

<style>
/* Global styles */
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f4f4f4;
}
</style>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
