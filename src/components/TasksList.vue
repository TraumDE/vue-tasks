<script lang="ts" setup>
import type { Task } from '@/types'

const props = defineProps<{
  tasks: Task[]
}>()
</script>

<template>
  <div class="tasks-list">
    <article v-for="task in props.tasks" :key="task.id">
      {{ task.title }}
    </article>
  </div>
</template>

<style scoped>
.tasks-list {
  margin-top: 1rem;
}
</style>
