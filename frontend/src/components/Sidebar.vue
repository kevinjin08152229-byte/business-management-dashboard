<script setup lang="ts">
const menuItems = [
  { name: 'Dashboard', icon: '📊', path: '/' },
  { name: 'Sales', icon: '💰', path: '/sales' },
  { name: 'Employees', icon: '👥', path: '/employees' },
  { name: 'Inventory', icon: '📦', path: '/inventory' },
  { name: 'Reports', icon: '📈', path: '/reports' },
  { name: 'Settings', icon: '⚙️', path: '/settings' },
]
</script>

<template>
  <aside class="hidden w-64 shrink-0 bg-slate-900 p-6 text-white md:block">

    <h1 class="mb-10 text-2xl font-bold">
      BizManager
    </h1>

    <nav class="space-y-2">
     <RouterLink
  v-for="item in menuItems"
  :key="item.name"
  :to="item.path"
  class="flex items-center gap-3 rounded-xl px-4 py-3 text-slate-600 transition hover:bg-blue-50 hover:text-blue-600"
  active-class="bg-blue-600 text-white hover:bg-blue-700 hover:text-white"
>
  <span>{{ item.icon }}</span>
  <span>{{ item.name }}</span>
</RouterLink>
    </nav>

  </aside>
</template>

