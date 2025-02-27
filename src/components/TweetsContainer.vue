<template>
  <div
    class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
  >
    <div
      class="w-11/12 max-w-4xl max-h-[80vh] overflow-y-auto bg-white dark:bg-gray-800 p-6 rounded-xl shadow-lg relative"
    >
      <!-- Close Button -->
      <button
        @click="$emit('close')"
        class="absolute top-4 right-4 text-gray-600 dark:text-gray-300 hover:text-red-500 text-2xl"
      >
        ✖
      </button>

      <h2 class="text-2xl font-bold mb-6 text-center">User Tweets</h2>

      <div v-if="tweetsData && tweetsData.tweet">
        <ul>
          <li
            v-for="(tweet, index) in Object.keys(tweetsData.tweet)"
            :key="index"
            class="border-b border-gray-300 dark:border-gray-700 py-4"
          >
            <p class="text-lg font-semibold">📝 {{ tweetsData.tweet[index] }}</p>
            <p class="text-sm text-gray-500 dark:text-gray-400 mt-1">
              🕒 {{ tweetsData.date_time[index] }}
            </p>
            <p class="text-sm mt-1">
              📊 Sentiment:
              <span
                :class="{
                  'text-green-500': tweetsData.sentiment[index] === 'positive',
                  'text-yellow-500': tweetsData.sentiment[index] === 'neutral',
                  'text-red-500': tweetsData.sentiment[index] === 'negative',
                }"
              >
                {{ tweetsData.sentiment[index] }}
              </span>
            </p>
            <p class="text-sm mt-1">
              🤖 AI Sentiment:
              <span
                :class="{
                  'text-green-500': tweetsData.distilbert_sentiment[index] === 'joy',
                  'text-red-500': tweetsData.distilbert_sentiment[index] === 'anger',
                  'text-blue-500': tweetsData.distilbert_sentiment[index] === 'fear',
                }"
              >
                {{ tweetsData.distilbert_sentiment[index] }}
              </span>
            </p>
          </li>
        </ul>
      </div>
      <p v-else class="text-gray-500 text-xl text-center">No tweets found.</p>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'

defineProps({
  tweetsData: Object,
})

defineEmits(['close'])
</script>
