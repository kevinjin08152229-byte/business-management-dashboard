
<script setup lang="ts">
import { ref, computed } from 'vue'
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Tooltip,
  Filler,
} from 'chart.js'

import { Line } from 'vue-chartjs'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Tooltip,
  Filler
)

// Selected chart period
const selectedPeriod = ref('yearly')

// Demo revenue data
const yearlyRevenue = [
  12000, 18000, 15000, 22000,
  28000, 25000, 32000, 38000,
  35000, 42000, 46000, 52000,
]

const monthlyRevenue = [
  1200, 1800, 1500, 2400,
  2100, 3200, 2800,
  3900, 3500, 4200,
  4800, 5200,
]

const quarterlyRevenue = [
  1200, 1800, 1500, 2400,

]
const chartData = computed(() => {
  let labels: string[]
  let revenue: number[]

  if (selectedPeriod.value === 'yearly') {
    labels = [
      'Jan', 'Feb', 'Mar', 'Apr',
      'May', 'Jun', 'Jul', 'Aug',
      'Sep', 'Oct', 'Nov', 'Dec',
    ]
    revenue = yearlyRevenue

  } else if (selectedPeriod.value === 'monthly') {
    labels = [
      'Week 1', 'Week 2', 'Week 3', 'Week 4',
      'Week 5', 'Week 6', 'Week 7', 'Week 8',
      'Week 9', 'Week 10', 'Week 11', 'Week 12',
    ]
    revenue = monthlyRevenue

  } else {
    labels = ['Q1', 'Q2', 'Q3', 'Q4']
    revenue = quarterlyRevenue
  }

  return {
    labels,
    datasets: [
      {
        label: 'Revenue',
        data: revenue,
        borderColor: '#3b82f6',
        backgroundColor: 'rgba(59, 130, 246, 0.12)',
        borderWidth: 3,
        fill: true,
        tension: 0.4,
        pointRadius: 4,
        pointHoverRadius: 7,
      },
    ],
  }
})

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,

  plugins: {
    legend: {
      display: false,
    },
    tooltip: {
      callbacks: {
        label: (context: any) =>
          `Revenue: $${context.parsed.y.toLocaleString()}`,
      },
    },
  },

  scales: {
    y: {
      beginAtZero: true,
      ticks: {
        callback: (value: number | string) => `$${Number(value) / 1000}k`,
      },
      grid: {
        color: '#f1f5f9',
      },
    },
    x: {
      grid: {
        display: false,
      },
    },
  },
}
</script>

<template>
  <section class="rounded-2xl bg-white p-6 shadow-sm border border-purple-500">

    <div class="mb-6 flex flex-wrap items-center justify-between gap-4">
      <div>
        <h3 class="text-xl font-bold">Revenue Analytics</h3>
        <p class="text-sm text-slate-500">
          Track your business revenue performance
        </p>
      </div>

      <select
        v-model="selectedPeriod"
        class="rounded-lg border border-slate-200 bg-white px-4 py-2 text-sm outline-none focus:border-blue-500"
      >
        <option value="yearly">Yearly View</option>
        <option value="monthly">Monthly View</option>
        <option value="quarterly">Quarterly View</option>
      </select>
    </div>

    <div class="h-72">
      <Line
        :data="chartData"
        :options="chartOptions"
      />
    </div>

  </section>
</template>
