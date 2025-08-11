<template>
  <div class="bg-white rounded-lg shadow p-6">
    <h2 class="text-lg font-semibold mb-4">Add Transaction</h2>
    <form @submit.prevent="handleSubmit" class="space-y-4">
      <input
        v-model="text"
        type="text"
        placeholder="Transaction Title"
        class="w-full border rounded p-2"
        required
      />
      <input
        v-model.number="amount"
        type="number"
        placeholder="Amount (+ for income, - for expense)"
        class="w-full border rounded p-2"
        required
      />
      <button
        type="submit"
        class="w-full bg-blue-500 text-white py-2 rounded hover:bg-blue-600"
      >
        Add Transaction
      </button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text: '',
      amount: null
    }
  },
  methods: {
    handleSubmit() {
      const transaction = {
        id: Date.now(),
        text: this.text,
        amount: this.amount
      }
      this.$emit('add', transaction)
      this.text = ''
      this.amount = null
    }
  }
}
</script>
