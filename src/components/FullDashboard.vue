<template>
  <div class="w-screen h-screen flex flex-col dark:bg-gray-900 dark:text-white bg-gray-200">
    <!-- Header-like section -->
    <div
      class="w-full h-20 flex items-center justify-between px-8 bg-white dark:bg-gray-800 shadow-md"
    >
      <h1 class="text-3xl font-bold">Tweets of {{ username ? username : '?' }}</h1>
      <button
        @click="showAllTweets"
        class="px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-700"
      >
        Show Tweets
      </button>
    </div>

    <!-- Main content area -->
    <div class="flex-grow w-full grid grid-cols-3 gap-4 p-6">
      <!-- Doughnut Chart -->
      <div class="bg-white dark:bg-gray-800 p-6 rounded-lg shadow-md flex flex-col items-center">
        <h2 class="text-xl font-semibold mb-4">Tweets per Sentiment</h2>
        <canvas ref="doughnutChart"></canvas>
      </div>
      <!-- Other chart divs here -->
    </div>

    <!-- Tweets Popup -->
    <TweetsContainer
      v-if="showingTweets"
      :tweetsData="filteredTweets"
      @close="showingTweets = false"
    />
  </div>
</template>

<script setup>
import { defineProps, ref, onMounted } from 'vue'
import TweetsContainer from './TweetsContainer.vue'
import Chart from 'chart.js/auto'

const showingTweets = ref(false)
const doughnutChart = ref(null)
const filteredTweets = ref({})

const props = defineProps({
  tweetsData: Object,
  username: String,
})

const showAllTweets = () => {
  filteredTweets.value = props.tweetsData
  showingTweets.value = true
}

const showFilteredTweets = (sentiment) => {
  const filteredKeys = Object.keys(props.tweetsData.sentiment || {}).filter(
    (key) => props.tweetsData.sentiment[key] === sentiment,
  )

  filteredTweets.value = filteredKeys.reduce((acc, key) => {
    Object.keys(props.tweetsData).forEach((field) => {
      if (!acc[field]) acc[field] = {}
      acc[field][key] = props.tweetsData[field][key]
    })
    return acc
  }, {})

  showingTweets.value = true
}

onMounted(() => {
  if (doughnutChart.value) {
    const sentimentCounts = {
      positive: Object.values(props.tweetsData.sentiment || {}).filter((s) => s === 'positive')
        .length,
      neutral: Object.values(props.tweetsData.sentiment || {}).filter((s) => s === 'neutral')
        .length,
      negative: Object.values(props.tweetsData.sentiment || {}).filter((s) => s === 'negative')
        .length,
    }

    const totalTweets =
      sentimentCounts.positive + sentimentCounts.neutral + sentimentCounts.negative

    const dataValues = [sentimentCounts.positive, sentimentCounts.neutral, sentimentCounts.negative]
    const labels = [
      `Positive (${((sentimentCounts.positive / totalTweets) * 100).toFixed(1)}%)`,
      `Neutral (${((sentimentCounts.neutral / totalTweets) * 100).toFixed(1)}%)`,
      `Negative (${((sentimentCounts.negative / totalTweets) * 100).toFixed(1)}%)`,
    ]

    const chartInstance = new Chart(doughnutChart.value, {
      type: 'doughnut',
      data: {
        labels: labels,
        datasets: [
          {
            data: dataValues,
            backgroundColor: ['#10B981', '#FBBF24', '#EF4444'],
            hoverOffset: 4,
          },
        ],
      },
      options: {
        onClick: (event, elements) => {
          if (elements.length > 0) {
            const index = elements[0].index
            const sentimentTypes = ['positive', 'neutral', 'negative']
            showFilteredTweets(sentimentTypes[index])
          }
        },
      },
    })
  }
})
</script>
