<script setup lang="ts">
import { ref, computed } from 'vue'
import TaskForm from './components/TaskForm.vue'
import type { Task } from './types'
import TasksList from './components/TasksList.vue'

const message = ref('Task App')
const tasks = ref<Task[]>([])

const totalDone = computed(() =>
  tasks.value.reduce((total, task) => (task.done ? total + 1 : total), 0),
)

const addTask = (newTask: string): void => {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  })
}

const toggleTaskDone = (id: string): void => {
  const task = tasks.value.find((task) => task.id === id)

  if (task) task.done = !task.done
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <task-form @add-task="addTask" />
    <h3 v-if="!tasks.length">Add task to get started</h3>
    <h3 v-else>{{ totalDone }} / {{ tasks.length }} tasks completed</h3>
    <!-- <h3>There are {{ tasks.length }} tasks</h3> -->
    <tasks-list :tasks @toggle-done="toggleTaskDone" />
  </main>
</template>

<style scoped>
main {
  max-width: 800px;
  margin-inline: auto;
}
</style>
