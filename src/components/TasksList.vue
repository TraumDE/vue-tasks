<script lang="ts" setup>
import type { Task } from '@/types'

const props = defineProps<{
  tasks: Task[]
}>()

const emit = defineEmits<{
  toggleDone: [id: string]
}>()
</script>

<template>
  <div class="tasks-list">
    <article v-for="task in props.tasks" :key="task.id">
      <label for="">
        <input :checked="task.done" @input="emit('toggleDone', task.id)" type="checkbox" />
        {{ task.title }}
      </label>
    </article>
  </div>
</template>

<style scoped>
.tasks-list {
  margin-top: 1rem;
}
</style>
