<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits<{
  addTask: [newTask: string]
}>()

const newTask = ref('')
const error = ref('')

const formSubmit = (): void => {
  if (newTask.value.trim()) {
    emit('addTask', newTask.value)
    error.value = ''
  } else {
    error.value = 'Please enter a task'
  }

  newTask.value = ''
}
</script>

<template>
  <form @submit.prevent="formSubmit" action="POST">
    <label for="newTask">
      New Task
      <input
        :aria-invalid="!!error || undefined"
        aria-describedby="invalid-helper"
        autocomplete="off"
        v-model="newTask"
        name="newTask"
        id="newTask"
        type="text"
        @input="newTask.trim() ? (error = '') : (error = 'Please enter a task')"
      />
      <small v-if="error" id="invalid-helper">
        {{ error }}
      </small>
    </label>
    <button class="add-task-button" type="submit" aria-label="add task">Add</button>
  </form>
</template>
