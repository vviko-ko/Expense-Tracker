<template>
  <div class="p-4 sm:p-6">
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">

      <!-- Transaction History -->
      <div class="bg-white p-4 rounded-xl shadow-lg border border-gray-200">
        <h2 class="text-lg sm:text-xl font-semibold mb-4 border-b border-gray-200 pb-2">
          Transaction History
        </h2>
        <div class="overflow-x-auto">
          <table class="min-w-full border-collapse text-sm sm:text-base">
            <thead class="bg-gradient-to-r from-gray-100 to-gray-200">
              <tr>
                <th class="py-3 px-4 text-left font-semibold text-gray-700 border-b border-gray-300">Title</th>
                <th class="py-3 px-4 text-left font-semibold text-gray-700 border-b border-gray-300">Amount</th>
                <th class="py-3 px-4 text-center font-semibold text-gray-700 border-b border-gray-300">Action</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="t in transactions.filter(t => !t.isSubscription)"
                :key="t.id"
                class="hover:bg-gray-50 transition"
              >
                <td class="py-3 px-4 border-b border-gray-200">{{ t.description }}</td>
                <td
                  class="py-3 px-4 border-b border-gray-200 font-medium"
                  :class="t.amount > 0 ? 'text-green-600' : 'text-red-600'"
                >
                  {{ t.amount }}
                </td>
                <td class="py-3 px-4 border-b border-gray-200 text-center">
                  <button
                    @click="$emit('delete', t.id)"
                    class="bg-red-500 text-white px-3 py-1.5 rounded-md hover:bg-red-600 shadow-sm transition"
                  >
                    Delete
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <!-- Subscription History -->
      <div class="bg-white p-4 rounded-xl shadow-lg border border-gray-200">
        <h2 class="text-lg sm:text-xl font-semibold mb-4 border-b border-gray-200 pb-2">
          Subscription History
        </h2>
        <div class="overflow-x-auto">
          <table class="min-w-full border-collapse text-sm sm:text-base">
            <thead class="bg-gradient-to-r from-gray-100 to-gray-200">
              <tr>
                <th class="py-3 px-4 text-left font-semibold text-gray-700 border-b border-gray-300">Title</th>
                <th class="py-3 px-4 text-left font-semibold text-gray-700 border-b border-gray-300">Amount</th>
                <th class="py-3 px-4 text-left font-semibold text-gray-700 border-b border-gray-300">Renewal Date</th>
                <th class="py-3 px-4 text-center font-semibold text-gray-700 border-b border-gray-300">Action</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="t in transactions.filter(t => t.isSubscription)"
                :key="t.id"
                class="hover:bg-gray-50 transition"
              >
                <td class="py-3 px-4 border-b border-gray-200">{{ t.description }}</td>
                <td
                  class="py-3 px-4 border-b border-gray-200 font-medium"
                  :class="t.amount > 0 ? 'text-green-600' : 'text-red-600'"
                >
                  {{ t.amount }}
                </td>
                <td class="py-3 px-4 border-b border-gray-200">
                  {{ new Date(t.renewalDate).toLocaleDateString() }}
                </td>
                <td class="py-3 px-4 border-b border-gray-200 text-center">
                  <button
                    @click="$emit('delete', t.id)"
                    class="bg-red-500 text-white px-3 py-1.5 rounded-md hover:bg-red-600 shadow-sm transition"
                  >
                    Delete
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  props: {
    transactions: {
      type: Array,
      default: () => []
    }
  }
}
</script>
