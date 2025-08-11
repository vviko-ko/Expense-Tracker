<template>
  <div class="bg-white  rounded-lg shadow p-6 max-w-md mx-auto">
    <h2 class="text-xl font-semibold mb-4 text ">Add Transaction</h2>

    <!-- Toggle Income / Expense -->
    <div class="flex mb-4">
      <button
        @click="type = 'income'"
        :class="toggleBtnClass(type === 'income')"
        class="flex-1 py-2 rounded-l "
      >
        Income
      </button>
      <button
        @click="type = 'expense'"
        :class="toggleBtnClass(type === 'expense')"
        class="flex-1 py-2 rounded-r "
      >
        Expense
      </button>
    </div>

    <form @submit.prevent="submitTransaction" class="space-y-4 text ">
      <div>
        <label class="block mb-1 font-medium">Description</label>
        <input
          v-model="description"
          type="text"
          placeholder="E.g., Salary, Groceries"
          required
          class="w-full px-3 py-2 rounded border border-gray-300  focus:outline-none focus:ring-2 focus:ring-indigo-500 "
        />
      </div>

      <div>
        <label class="block mb-1 font-medium">Amount</label>
        <input
          v-model.number="amount"
          type="number"
          step="0.01"
          placeholder="E.g., 1000"
          required
          class="w-full px-3 py-2 rounded border border-gray-300  focus:outline-none focus:ring-2 focus:ring-indigo-500"
        />
        <small class="text-gray-500 dark:text-gray-400">
          Enter positive number; sign will be adjusted automatically.
        </small>
      </div>

      <div>
        <label class="block mb-1 font-medium">Category</label>
        <input
          v-model="category"
          type="text"
          placeholder="E.g., Salary, Food, Transport"
          class="w-full px-3 py-2 rounded border border-gray-300  focus:outline-none focus:ring-2 focus:ring-indigo-500 "
        />
      </div>

      <button
        type="submit"
        class="w-full bg-blue-600 text-white font-semibold py-2 rounded"
      >
        Add {{ type === 'income' ? 'Income' : 'Expense' }}
      </button>
    </form>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  emits: ['add'],
  setup(props, { emit }) {
    const type = ref('income')
    const description = ref('')
    const amount = ref(null)
    const category = ref('')

    function submitTransaction() {
      if (!description.value || !amount.value) return

      const newTransaction = {
        id: Date.now(),
        description: description.value.trim(),
        amount: type.value === 'income' ? Math.abs(amount.value) : -Math.abs(amount.value),
        category: category.value.trim() || (type.value === 'income' ? 'Income' : 'Expense'),
        date: new Date().toISOString(),
      }

      emit('add', newTransaction)

      // reset form
      description.value = ''
      amount.value = null
      category.value = ''
    }

    function toggleBtnClass(selected) {
  return selected
    ? 'bg-blue-600 text-white font-semibold'
    : 'bg-white text-blue-600 hover:bg-blue-100'
}

    return { type, description, amount, category, submitTransaction, toggleBtnClass }
  }
}
</script>

<style scoped>
/* add scoped styles if needed */
</style>
