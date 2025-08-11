<template>
  <table class="min-w-full border">
    <thead class="bg-gray-100">
      <tr>
        <th class="py-2 px-4 border">Title</th>
        <th class="py-2 px-4 border">Amount</th>
        <th class="py-2 px-4 border">Action</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="t in transactions" :key="t.id">
        <td class="py-2 px-4 border">{{ t.text }}</td>
        <td
          class="py-2 px-4 border"
          :class="t.amount > 0 ? 'text-green-600' : 'text-red-600'"
        >
          {{ t.amount }}
        </td>
        <td class="py-2 px-4 border text-center">
          <button
            @click="$emit('delete', t.id)"
            class="bg-red-500 text-white px-2 py-1 rounded hover:bg-red-600"
          >
            Delete
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    transactions: Array
  }
}
</script>
