<template>
  <button @click="showTimer = !showTimer">Afficher / masquer</button>
  <Timer v-if="showTimer"></Timer>
  <form action="" @submit.prevent="addTodo">
    <fieldset role="group">
      <input
          v-model="newTodo"
          placeholder="Tâche à effectuer"
          type="text"
      >
      <button :disabled="newTodo.length === 0">Ajouter</button>
    </fieldset>
  </form>
  <div v-if="todos.length === 0">Vous n'avez pas de tâches à faire :(</div>
  <div v-else>
    <ul>
      <li
          v-for="todo in sortedTodos"
          :key="todo.date"
          :class="{completed: todo.completed}"
      >
        <label>
          <Checkbox :label="todo.title"></Checkbox>
        </label>
      </li>
    </ul>
    <label>
      <input v-model="hideCompleted" type="checkbox">
      Masquer les tâches complétées
    </label>

    <p v-if="remainingTodos > 0">
      {{ remainingTodos }} tâche{{ remainingTodos > 1 ? 's' : '' }} à faire
    </p>
    <Checkbox :label="`Bonjour`"></Checkbox>
  </div>
</template>

<script setup>
import {computed, onMounted, ref} from "vue";
import Checkbox from "@/Checkbox.vue";
import Button from "@/Button.vue";
import Layout from "@/Layout.vue";
import Timer from "@/Timer.vue";

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([])
const showTimer = ref(true)

onMounted(() => {
  fetch('https://jsonplaceholder.typicode.com/todos')
      .then(r => r.json())
      .then(v => todos.value = v.map(todo => ({...todo, date: todo.id})))
})
const addTodo = () => {
  todos.value.push({
    title: newTodo.value,
    completed: false,
    date: Date.now()
  })
  newTodo.value = ''
}
const sortedTodos = computed(() => {
  const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
  if (hideCompleted.value === true) {
    return sortedTodos.filter(t => t.completed === false)
  }
  return sortedTodos
})
const remainingTodos = computed(() => {
  return todos.value.filter(t => t.completed === false).length
})

</script>

<style>
.completed {
  opacity: .5;
  text-decoration: line-through;
}
</style>
