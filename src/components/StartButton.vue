<template>
  <div class="w-screen h-screen flex items-center justify-center bg-gray-200 dark:bg-gray-900 text-gray-900 dark:text-white">
    <div class="text-center">
      <h1 class="text-3xl font-bold mb-4">Search for a user</h1>
      <input
        type="text"
        v-model="username"
        class="border-2 border-gray-300 p-2 rounded-lg text-lg focus:outline-none focus:ring-2 focus:ring-blue-500 dark:bg-gray-800 dark:text-white"
        placeholder="Enter username"
      />
      <button
        @click="sendUsername"
        class="ml-2 px-4 py-2 bg-blue-500 text-white rounded-lg text-lg hover:bg-blue-600 transition dark:bg-blue-700"
      >
        Search
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const username = ref('')
const emit = defineEmits(['user-sent'])

const sendUsername = () => {
  emit('user-sent', username.value)
}
</script>
