
<script setup lang="ts">
import { ref } from 'vue'
import Sidebar from '../components/Sidebar.vue'
import StatCard from '../components/StatCard.vue'
import RevenueChart from '../components/RevenueChart.vue'

const stats = ref([
  {
    title: 'Total Revenue',
    value: '$65,000',
    change: '+12.5%',
    icon: '💰',
  },
  {
    title: 'Total Sales',
    value: '1,284',
    change: '+8.2%',
    icon: '🛒',
  },
  {
    title: 'Employees',
    value: '60',
    change: '+3.4%',
    icon: '👥',
  },
  {
    title: 'Products',
    value: '356',
    change: '+5.7%',
    icon: '📦',
  },
  {
    title: 'Total Orders',
    value: '850',
    change: '+5.7%',
    icon: '📦',
  }
])

const transactions = ref([
  { id: '#TRX-001', customer: 'John Smith', amount: '$250.00', status: 'Completed' },
  { id: '#TRX-002', customer: 'Emma Wilson', amount: '$180.00', status: 'Pending' },
  { id: '#TRX-003', customer: 'David Brown', amount: '$420.00', status: 'Completed' },
  { id: '#TRX-004', customer: 'Sophia Lee', amount: '$95.00', status: 'Cancelled' },
])

// const menuItems = [
//   'Dashboard',
//   'Sales',
//   'Employees',
//   'Inventory',
//   'Reports',
//   'Settings',
// ]
</script>

<template>
  <div class="min-h-screen bg-slate-100 text-slate-800">

    <div class="flex min-h-screen">

      <!-- Sidebar -->
     <Sidebar />

      <!-- Main content -->
      <main class="min-w-0 flex-1 p-6 md:p-8">

        <!-- Header -->
        <header class="mb-8 flex flex-wrap items-center justify-between gap-4">

          <div>
            <h2 class="text-3xl font-bold">
              Dashboard
            </h2>
            <p class="mt-1 text-slate-500">
              Welcome back! Here's your business overview.
            </p>
          </div>

          <div class="flex items-center gap-3">
            <span class="rounded-full bg-white p-3 shadow-sm">🔔</span>
            <div>
              <p class="font-semibold">Kevin Jin</p>
              <p class="text-sm text-slate-500">Administrator</p>
            </div>
          </div>

        </header>

        <!-- Statistics -->
        <section class="mb-8 grid grid-cols-1 gap-5 sm:grid-cols-2 xl:grid-cols-4">
          <StatCard 
            v-for="stat in stats"
            :key="stat.title"
            :title="stat.title"
            :value="stat.value"
            :change="stat.change"
            :icon="stat.icon"
          />
          <!-- <div
            v-for="stat in stats"
            :key="stat.title"
            class="rounded-2xl bg-white p-6 shadow-sm transition hover:-translate-y-1 hover:shadow-lg"
          >
            <div class="flex items-center justify-between">
              <p class="text-sm font-medium text-slate-500">
                {{ stat.title }}
              </p>
              <span class="text-2xl">{{ stat.icon }}</span>
            </div>

            <h3 class="mt-4 text-3xl font-bold">
              {{ stat.value }}
            </h3>

            <p class="mt-2 text-sm font-medium text-emerald-600">
              {{ stat.change }} this month
            </p>
          </div> -->

        </section>

        <!-- Sales overview -->
        <section class="mb-8 rounded-2xl bg-white p-6 shadow-sm">

         <RevenueChart />

        </section>

        <!-- Transactions -->
        <section class="overflow-hidden rounded-2xl bg-white shadow-sm">

          <div class="border-b border-slate-100 p-6">
            <h3 class="text-xl font-bold">Recent Transactions</h3>
            <p class="text-sm text-slate-500">Your latest business transactions</p>
          </div>

          <div class="overflow-x-auto">
            <table class="w-full text-left text-sm">

              <thead class="bg-slate-50 text-slate-500">
                <tr>
                  <th class="p-4">Transaction ID</th>
                  <th class="p-4">Customer</th>
                  <th class="p-4">Amount</th>
                  <th class="p-4">Status</th>
                </tr>
              </thead>

              <tbody>
                <tr
                  v-for="transaction in transactions"
                  :key="transaction.id"
                  class="border-t border-slate-100"
                >
                  <td class="p-4 font-medium">{{ transaction.id }}</td>
                  <td class="p-4">{{ transaction.customer }}</td>
                  <td class="p-4 font-semibold">{{ transaction.amount }}</td>
                  <td class="p-4">
                    <span
                      class="rounded-full px-3 py-1 text-xs font-medium"
                      :class="{
                        'bg-emerald-100 text-emerald-700': transaction.status === 'Completed',
                        'bg-amber-100 text-amber-700': transaction.status === 'Pending',
                        'bg-red-100 text-red-700': transaction.status === 'Cancelled'
                      }"
                    >
                      {{ transaction.status }}
                    </span>
                  </td>
                </tr>
              </tbody>

            </table>
          </div>

        </section>

      </main>
    </div>
  </div>
</template>
