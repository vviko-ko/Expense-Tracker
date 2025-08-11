<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Header -->
    <header class="bg-white shadow px-6 py-4 flex justify-between items-center">
      <h1 class="text-2xl font-bold text-gray-800">💰 Personal Expense Tracker</h1>
      <div class="text-gray-500 text-sm">
        Updated: {{ currentDate }}
      </div>
    </header>

    <!-- Summary Cards -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 p-6">
      <SummaryCard title="Balance" :amount="balance" type="balance" />
      <SummaryCard title="Income" :amount="income" type="income" />
      <SummaryCard title="Expenses" :amount="expenses" type="expense" />
    </div>

    <!-- Main Content -->
    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 px-6 pb-6">
      <div class="col-span-1">
        <AddTransaction @add="addTransaction" />
      </div>
      <div class="col-span-2 bg-white rounded-lg shadow p-6">
        <h2 class="text-lg font-semibold mb-4">Spending Overview</h2>
        <ExpenseChart :transactions="transactions" />
      </div>
    </div>

    <!-- Transactions Table -->
    <div class="px-6 pb-6">
      <div class="bg-white rounded-lg shadow p-6">
        <h2 class="text-lg font-semibold mb-4">Transaction History</h2>
        <TransactionTable :transactions="transactions" @delete="deleteTransaction" />
      </div>
    </div>
  </div>
</template>

<script>
import AddTransaction from './components/AddTransaction.vue'
import TransactionTable from './components/TransactionList.vue'
import SummaryCard from './components/SummaryCard.vue'
import ExpenseChart from './components/ExpenseChart.vue'

export default {
  components: { AddTransaction, TransactionTable, SummaryCard, ExpenseChart },
  data() {
    return {
      transactions: JSON.parse(localStorage.getItem('transactions')) || []
    }
  },
  computed: {
    currentDate() {
      return new Date().toLocaleDateString()
    },
    income() {
      return this.transactions
        .filter(t => t.amount > 0)
        .reduce((acc, t) => acc + t.amount, 0)
    },
    expenses() {
      return this.transactions
        .filter(t => t.amount < 0)
        .reduce((acc, t) => acc + t.amount, 0)
    },
    balance() {
      return this.income + this.expenses
    }
  },
  methods: {
    addTransaction(transaction) {
      this.transactions.push(transaction)
      this.saveData()
    },
    deleteTransaction(id) {
      this.transactions = this.transactions.filter(t => t.id !== id)
      this.saveData()
    },
    saveData() {
      localStorage.setItem('transactions', JSON.stringify(this.transactions))
    }
  }
}
</script>
