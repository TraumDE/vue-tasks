<script setup lang="ts">
import { ref, computed } from 'vue'
import TaskForm from './components/TaskForm.vue'
import type { Task, TaskFilter } from './types'
import TasksList from './components/TasksList.vue'
import FilterButton from './components/FilterButton.vue'

const message = ref('Task App')
const tasks = ref<Task[]>([])
const filter = ref<TaskFilter>('all')

const totalDone = computed(() =>
  tasks.value.reduce((total, task) => (task.done ? total + 1 : total), 0),
)

const filteredTasks = computed(() => {
  switch (filter.value) {
    case 'all':
      return tasks.value
    case 'todo':
      return tasks.value.filter((task) => !task.done)
    case 'done':
      return tasks.value.filter((task) => task.done)
    default:
      return tasks.value
  }
})

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

const removeTask = (id: string): void => {
  const index = tasks.value.findIndex((task) => task.id === id)

  if (index !== -1) tasks.value.splice(index, 1)
}

const setFilter = (newFilter: TaskFilter): void => {
  if (filter.value !== newFilter) filter.value = newFilter
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <task-form @add-task="addTask" />
    <transition-group v-if="tasks.length" name="buttons" tag="div" class="button-container">
      <filter-button :currentFilter="filter" filter="all" @set-filter="setFilter"></filter-button>
      <filter-button :currentFilter="filter" filter="todo" @set-filter="setFilter"></filter-button>
      <filter-button :currentFilter="filter" filter="done" @set-filter="setFilter"></filter-button>
    </transition-group>
    <h3 v-if="!tasks.length">Add task to get started</h3>
    <h3 v-else>{{ totalDone }} / {{ tasks.length }} tasks completed</h3>
    <h3 v-if="tasks.length && !filteredTasks.length && filter === 'done'">
      You not have completed tasks
    </h3>
    <h3 v-else-if="tasks.length && !filteredTasks.length && filter === 'todo'">Nothing todo</h3>

    <tasks-list :tasks="filteredTasks" @remove-task="removeTask" @toggle-done="toggleTaskDone" />
  </main>
</template>

<style scoped>
main {
  max-width: 800px;
  margin-inline: auto;
}

.button-container {
  display: flex;
  flex-wrap: nowrap;
  gap: 10px;
  justify-content: end;
  margin-bottom: 10px;
}

.buttons-enter-active,
.buttons-end-active {
  transition: all 0.3s ease;
}

.buttons-enter-from,
.buttons-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
</style>
