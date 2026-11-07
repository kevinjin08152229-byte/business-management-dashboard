<script setup lang="ts">
import Sidebar from '../components/Sidebar.vue'

</script>
<template>
  <div class="min-h-screen bg-slate-100">
    <div class="flex min-h-screen">
      <Sidebar />
      <main class="min-w-0 flex-1 p-6 md:p-8">
      <h1 class="text-3xl font-bold text-slate-800">
         Sales Management</h1>
   
       <p class="mt-2 text-slate-500">
         Manage and monitor your business sales.
       </p>
   
       <div class="mt-8 grid gap-6 md:grid-cols-3">
         <div class="rounded-2xl bg-white p-6 shadow-sm">
           <p class="text-sm text-slate-500">Total Sales</p>
           <h2 class="mt-3 text-3xl font-bold">$48,250</h2>
         </div>
   
         <div class="rounded-2xl bg-white p-6 shadow-sm">
           <p class="text-sm text-slate-500">Orders</p>
           <h2 class="mt-3 text-3xl font-bold">1,284</h2>
         </div>
   
         <div class="rounded-2xl bg-white p-6 shadow-sm">
           <p class="text-sm text-slate-500">Average Order</p>
           <h2 class="mt-3 text-3xl font-bold">$37.58</h2>
         </div>
       </div>
      </main>
    </div>
  </div>
</template>