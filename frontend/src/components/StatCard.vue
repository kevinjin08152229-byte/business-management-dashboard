
<script setup lang="ts">
interface Props {
  title: string
  value: string
  change: string
  icon: string
}

defineProps<Props>()
</script>

<template>
  <div class="rounded-2xl bg-white p-6 shadow-sm transition hover:-translate-y-1 hover:shadow-lg">

    <div class="flex items-center justify-between">
      <p class="text-sm font-medium text-slate-500">
        {{ title }}
      </p>

      <span class="text-2xl">{{ icon }}</span>
    </div>

    <h3 class="mt-4 text-3xl font-bold">
      {{ value }}
    </h3>

    <p class="mt-2 text-sm font-medium text-emerald-600">
      {{ change }} this month
    </p>

  </div>
</template>
