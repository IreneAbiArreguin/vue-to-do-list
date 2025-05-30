<script lang="ts" setup>
defineProps<{
  isChecked?: boolean | false;
}>();
</script>

<template>
  <label :class="{ checked: isChecked }" class="todo-label">
    <input type="checkbox" :checked="isChecked" class="todo-checkbox" />
    <span class="todo-text"><slot></slot></span>
  </label>
</template>

<style scoped>
.todo-label {
  cursor: pointer;
  display: flex;
  align-items: center;
  padding: 0.6rem 1rem;
  border-radius: 8px;
  background: #f8fafc;
  transition: background 0.2s;
  box-shadow: 0 1px 4px rgba(0,0,0,0.04);
  width: 100%;
}

.todo-label:hover {
  background: #e0e7ef;
}

.todo-checkbox {
  accent-color: #4f8cff;
  margin-right: 1rem;
  width: 1.2rem;
  height: 1.2rem;
}

.todo-text {
  font-size: 1.1rem;
  color: #222;
  transition: color 0.2s;
}

.checked .todo-text {
  text-decoration: line-through;
  color: #b0b0b0;
}
</style>
