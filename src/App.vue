<script setup lang="ts">
import Header from '@/components/Header.vue'
import Balance from '@/components/Balance.vue'
import IncomeExpenses from '@/components/IncomeExpenses.vue'
import TransactionLis from '@/components/TransactionLis.vue'
import FormAdd from '@/components/FormAdd.vue'
import type { Transactions } from '@/components/TransactionLis.vue'
import { useToast } from 'vue-toastification'

const toast = useToast()

import { ref, computed, onMounted, watch } from 'vue'

const transactions = ref<Transactions[]>([])

onMounted(() => {
  const savedTransactions = localStorage.getItem('transactions')
  if (savedTransactions !== null) {
    const parsedTransactions: Transactions[] = JSON.parse(savedTransactions)
    // Proceed with the parsedTransactions
    transactions.value = parsedTransactions
  }
})

// Compute the total amount of all transactions
const total = computed(() => {
  return transactions.value.reduce((acc, curr) => acc + curr.amount, 0)
})

const totalIncome = computed(() => {
  return transactions.value
    .filter((trans) => trans.amount > 0)
    .reduce((acc, curr) => acc + curr.amount, 0)
    .toFixed(2)
})
const totalWaste = computed(() => {
  return (
    transactions.value
      .filter((trans) => trans.amount < 0)
      .reduce((acc, curr) => acc + curr.amount, 0) * -1
  ).toFixed(2)
})

const eliminateTransition = (id: number) => {
  const filterTransitions = transactions.value.filter((trans) => trans.id !== id)
  transactions.value = filterTransitions

  toast.success('Transaction deleted successfully')
}

const handleTransactionSubmitted = (transactionData: { text: string; amount: number }) => {
  const id = transactions.value.length + 1
  const { text, amount } = transactionData
  const addNewTransaction = { id, text, amount }
  transactions.value = [...transactions.value, addNewTransaction]

  toast.success('Transaction added successfully')
}

// Watch for changes in the transactions.value array
watch(
  transactions,
  (newTransactions) => {
    localStorage.setItem('transactions', JSON.stringify(newTransactions))
  },
  { deep: true }
)
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+totalIncome" :expense="+totalWaste" />
    <TransactionLis :transactions="transactions" :eliminateTransactions="eliminateTransition" />
    <FormAdd @submitted="handleTransactionSubmitted" />
  </div>
</template>
