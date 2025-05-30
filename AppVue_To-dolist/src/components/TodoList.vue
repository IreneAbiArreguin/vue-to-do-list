<script lang="ts" setup>
import { ref, onMounted, computed } from 'vue'
import type { Ref } from 'vue'
import ListItem from './ListItem.vue'

type Item = { id: number, title: string; checked?: boolean }

const storageItems: Ref<Item[]> = ref([])
const newItemTitle = ref('')

const fetchItems = async () => {
  const res = await fetch('http://localhost:3000/items')
  const data = await res.json()
  storageItems.value = data
}

const initListItems = async () => {
  await fetchItems()
  if (storageItems.value.length === 0) {
    const initialItems = [
      { title: 'Make a todo list app', checked: true },
      { title: 'Predict the weather', checked: false },
      { title: 'Read some comics', checked: false },
      { title: "Let's get cooking", checked: false },
      { title: 'Pump some iron', checked: false },
      { title: 'Track my expenses', checked: false },
      { title: 'Organise a game night', checked: false },
      { title: 'Learn a new language', checked: false },
      { title: 'Publish my work' }
    ]
    for (const item of initialItems) {
      await saveItemJson(item)
    }
    await fetchItems()
  }
}

const sortedList = computed(() =>
  [...storageItems.value].sort((a, b) => (a.checked ? 1 : 0) - (b.checked ? 1 : 0))
)

const updateItem = async (item: Item) => {
  const updatedItem = { ...item, checked: !item.checked }
  await updateItemJson(updatedItem)
  await fetchItems()
}

const saveItemJson = async (item: Omit<Item, 'id'>) => {
  await fetch('http://localhost:3000/items', {
    method: 'POST',
    body: JSON.stringify(item),
    headers: {
      'Content-type': 'application/json; charset=UTF-8',
    },
  })
}

const updateItemJson = async (item: Item) => {
  await fetch(`http://localhost:3000/items/${item.id}`, {
    method: 'PUT',
    body: JSON.stringify(item),
    headers: {
      'Content-type': 'application/json; charset=UTF-8',
    },
  })
}

const deleteItem = async (item: Item) => {
  await fetch(`http://localhost:3000/items/${item.id}`, {
    method: 'DELETE',
  })
  await fetchItems()
}

const addItem = async () => {
  const title = newItemTitle.value.trim()
  if (!title) return
  await saveItemJson({ title, checked: false })
  newItemTitle.value = ''
  await fetchItems()
}

onMounted(async () => {
  await initListItems()
})
</script>

<template>
  <div class="todo-container">
    <form @submit.prevent="addItem" class="todo-form">
      <input v-model="newItemTitle" placeholder="Nueva tarea" class="todo-input" />
      <button type="submit" class="todo-add-btn">Agregar</button>
    </form>
    <ul class="todo-list">
      <li :key="item.id" v-for="item in sortedList" class="todo-list-item">
        <ListItem :is-checked="item.checked" @click="updateItem(item)">
          {{ item.title }}
        </ListItem>
        <button @click="deleteItem(item)" class="todo-delete-btn">Eliminar</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.todo-container {
  max-width: 420px;
  margin: 2rem auto;
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 4px 24px rgba(0,0,0,0.08);
  padding: 2rem 1.5rem 1.5rem 1.5rem;
}

.todo-form {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}

.todo-input {
  flex: 1;
  padding: 0.7rem 1rem;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
  transition: border 0.2s;
}

.todo-input:focus {
  border-color: #4f8cff;
}

.todo-add-btn {
  background: #4f8cff;
  color: #fff;
  border: none;
  border-radius: 8px;
  padding: 0.7rem 1.2rem;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.2s;
}

.todo-add-btn:hover {
  background: #2563eb;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.todo-list-item {
  display: flex;
  align-items: center;
  margin-bottom: 0.7rem;
  gap: 0.5rem;
}

.todo-delete-btn {
  background: #ff4f4f;
  color: #fff;
  border: none;
  border-radius: 6px;
  padding: 0.5rem 0.9rem;
  font-size: 0.95rem;
  cursor: pointer;
  margin-left: 0.5rem;
  transition: background 0.2s;
}

.todo-delete-btn:hover {
  background: #d90429;
}
</style>
