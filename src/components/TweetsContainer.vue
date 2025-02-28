<template>
  <div class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
    <div
      class="w-11/12 max-w-4xl max-h-[80vh] overflow-y-auto bg-white dark:bg-gray-800 p-6 rounded-xl shadow-lg relative"
    >
      <button
        @click="$emit('close')"
        class="absolute top-4 right-4 text-gray-600 dark:text-gray-300 hover:text-red-500 text-2xl"
      >
        ✖
      </button>

      <h2 class="text-2xl font-bold mb-6 text-center">User Tweets</h2>

      <div v-if="allKeys.length > 0">
        <ul>
          <li
            v-for="tweetId in allKeys"
            :key="tweetId"
            class="border-b border-gray-300 dark:border-gray-700 py-4"
          >
            <p class="text-lg font-semibold">
              📝 {{ tweetsData.tweet?.[tweetId] ?? 'No text' }}
            </p>

            <p class="text-sm text-gray-500 dark:text-gray-400 mt-1">
              🕒 {{ tweetsData.date_time?.[tweetId] ?? 'No date' }}
            </p>

            <p class="text-sm mt-1">
              📊 Sentiment:
              <span
                :class="{
                  'text-green-500': tweetsData.sentiment?.[tweetId] === 'positive',
                  'text-yellow-500': tweetsData.sentiment?.[tweetId] === 'neutral',
                  'text-red-500': tweetsData.sentiment?.[tweetId] === 'negative',
                }"
              >
                {{ tweetsData.sentiment?.[tweetId] ?? 'No sentiment' }}
              </span>
            </p>

            <p class="text-sm mt-1">
              🤖 AI Sentiment:
              <span
                :class="{
                  'text-green-500': tweetsData.distilbert_sentiment?.[tweetId] === 'joy',
                  'text-red-500': tweetsData.distilbert_sentiment?.[tweetId] === 'anger',
                  'text-blue-500': tweetsData.distilbert_sentiment?.[tweetId] === 'fear',
                }"
              >
                {{ tweetsData.distilbert_sentiment?.[tweetId] ?? 'No AI sentiment' }}
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
import { computed, defineProps, defineEmits } from 'vue'

defineEmits(['close'])

const props = defineProps({
  tweetsData: {
    type: Object,
    default: () => ({})
  }
})

const allKeys = computed(() => {
  const tweetKeys = Object.keys(props.tweetsData.tweet || {})
  const dateKeys = Object.keys(props.tweetsData.date_time || {})
  const sentimentKeys = Object.keys(props.tweetsData.sentiment || {})
  const distilbertKeys = Object.keys(props.tweetsData.distilbert_sentiment || {})

  const uniqueKeys = new Set([...tweetKeys, ...dateKeys, ...sentimentKeys, ...distilbertKeys])
  return Array.from(uniqueKeys)
})
</script>
