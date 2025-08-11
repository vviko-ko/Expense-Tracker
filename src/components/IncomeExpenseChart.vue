<template>
  <div class="bg-white p-4 rounded-lg shadow-md">
    <h2 class="text-lg font-bold mb-4">Income vs Expenses</h2>
    <Bar :data="chartData" :options="chartOptions" />
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  components: { Bar },
  props: ["transactions"],
  computed: {
    chartData() {
      const months = Array.from({ length: 12 }, (_, i) =>
        new Date(0, i).toLocaleString('default', { month: 'short' })
      )
      const incomeData = new Array(12).fill(0)
      const expenseData = new Array(12).fill(0)

      this.transactions.forEach(t => {
        const month = new Date(t.date).getMonth()
        if (t.amount > 0) {
          incomeData[month] += t.amount
        } else {
          expenseData[month] += Math.abs(t.amount)
        }
      })

      return {
        labels: months,
        datasets: [
          { label: 'Income', backgroundColor: '#10b981', data: incomeData },
          { label: 'Expenses', backgroundColor: '#ef4444', data: expenseData }
        ]
      }
    },
    chartOptions() {
      return { responsive: true, plugins: { legend: { position: 'bottom' } } }
    }
  }
}
</script>
