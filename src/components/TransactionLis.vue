<script setup lang="ts">
import { ref, computed } from 'vue'
import { toastInjectionKey } from 'vue-toastification'
export interface Transactions {
  id: number
  text: string
  amount: number
}

interface Props {
  transactions: Transactions[]
  eliminateTransactions: (id: number) => void
}
const id = ref(0)

const props = defineProps<Props>()

const transactions = computed(() => props.transactions.filter((trans) => trans.id !== id.value))

const eliminateTransaction = (chooseId: number) => {
  id.value = chooseId
  props.eliminateTransactions(chooseId)
}
</script>

<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="trans in transactions"
      :class="{ minus: trans.amount < 0, plus: trans.amount > 0 }"
      :key="trans.id"
    >
      {{ trans.text }} <span>$ {{ trans.amount }}</span
      ><button class="delete-btn" @click="eliminateTransaction(trans.id)">x</button>
    </li>
  </ul>
</template>

<style scoped></style>
