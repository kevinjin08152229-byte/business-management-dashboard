<script setup lang="ts">
const menuItems = [
  'Dashboard',
  'Sales',
  'Employees',
  'Inventory',
  'Reports',
  'Settings',
]
</script>

<template>
  <aside class="hidden w-64 shrink-0 bg-slate-900 p-6 text-white md:block">

    <h1 class="mb-10 text-2xl font-bold">
      BizManager
    </h1>

    <nav class="space-y-2">
      <a
        v-for="item in menuItems"
        :key="item"
        href="#"
        class="block rounded-lg px-4 py-3 text-slate-300 transition hover:bg-slate-700 hover:text-white"
        :class="{ 'bg-blue-600 text-white': item === 'Dashboard' }"
      >
        {{ item }}
      </a>
    </nav>

  </aside>
</template>

