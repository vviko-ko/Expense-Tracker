<template>
  <div class="bg-white shadow-md rounded-lg p-4 w-full max-w-md mx-auto">
    <div class="flex justify-between items-center mb-4">
      <h2 class="text-lg font-bold">Income vs Expenses</h2>
      <button
        @click="downloadReport"
        class="bg-blue-500 text-white px-3 py-1 rounded hover:bg-blue-600 text-sm"
      >
        Download Report
      </button>
    </div>
    <div class="h-64">
      <DoughnutChart :data="chartData" :options="chartOptions" />
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import { Chart as ChartJS, Title, Tooltip, Legend, ArcElement } from "chart.js";
import { Doughnut } from "vue-chartjs";
import jsPDF from "jspdf";

ChartJS.register(Title, Tooltip, Legend, ArcElement);

export default defineComponent({
  name: "ExpenseChart",
  components: {
    DoughnutChart: Doughnut
  },
  props: {
    transactions: {
      type: Array,
      required: true
    }
  },
  computed: {
    chartData() {
      const categories = {
        Income: 0,
        Expenses: 0
      };

      this.transactions.forEach((t) => {
        if (t.amount > 0) {
          categories.Income += t.amount;
        } else {
          categories.Expenses += Math.abs(t.amount);
        }
      });

      return {
        labels: ["Income", "Expenses"],
        datasets: [
          {
            label: "Income vs Expenses",
            data: [categories.Income, categories.Expenses],
            backgroundColor: [
              "#22c55e", // green for income
              "#ef4444"  // red for expenses
            ],
            borderWidth: 1
          }
        ]
      };
    },
    chartOptions() {
      return {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            position: "bottom"
          }
        }
      };
    }
  },
  methods: {
    downloadReport() {
      const doc = new jsPDF();
      doc.setFontSize(18);
      doc.text("Account Report", 14, 20);

      doc.setFontSize(12);
      doc.text(`Total Income: ₦${this.chartData.datasets[0].data[0]}`, 14, 40);
      doc.text(`Total Expenses: ₦${this.chartData.datasets[0].data[1]}`, 14, 50);

      doc.text("Transactions:", 14, 70);

      let y = 80;
      this.transactions.forEach((t) => {
        doc.text(`${t.date} - ${t.text} - ₦${t.amount}`, 14, y);
        y += 10;
      });

      doc.save("account_report.pdf");
    }
  }
});
</script>
