<script lang="ts" setup>
import type { Task } from '@/types'

const props = defineProps<{
  tasks: Task[]
}>()

const emits = defineEmits<{
  toggleDone: [id: string]
  removeTask: [id: string]
}>()
</script>

<template>
  <transition-group name="list" tag="div" class="task-list">
    <article v-for="task in props.tasks" class="task" :key="task.id">
      <label>
        <input :checked="task.done" @input="emits('toggleDone', task.id)" type="checkbox" />
        <span :class="{ 'task-list__text--done': task.done }">{{ task.title }} </span>
      </label>
      <button @click="emits('removeTask', task.id)" class="outline">Remove</button>
    </article>
  </transition-group>
</template>

<style scoped>
.tasks-list {
  margin-top: 1rem;
  width: 100%;
}

.task {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-list__text--done {
  text-decoration: line-through;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(300px);
}
</style>
