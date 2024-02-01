<template>
  <div class="container">
    <Bar v-if="loaded" :data="chartData" :options="chartOptions" />
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'BarChart',
  components: { Bar },
  data: () => ({
    loaded: false,
    chartData: null,
    chartOptions: {
      scales: {
        x: {
          stacked: false, // Set to false to place bars side by side
        },
        y: {
          beginAtZero: true,
        },
      },
      // Additional options like responsiveness can be added here
    },
  }),
  async mounted () {
    this.loaded = false
    try {
      const response = await fetch('http://localhost:8000/api/sendChartData')
      const data = await response.json()
      this.chartData = data
      this.loaded = true
    } catch (e) {
      console.error(e)
    }
  }
}
</script>
