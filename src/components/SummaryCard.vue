<template>
  <div class="bg-white rounded-lg shadow p-6 text-center">
    <h3 class="text-sm text-gray-500">{{ title }}</h3>
    <p
      class="text-2xl font-bold"
      :class="{
        'text-green-600': type === 'income',
        'text-red-600': type === 'expense',
        'text-blue-600': type === 'balance'
      }"
    >
      {{ amount }}
    </p>
  </div>
</template>

<script>
export default {
  props: ['title', 'amount', 'type']
}
</script>
