<script setup lang="ts">
import { ref } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()
const onSubmit = () => {
  if (!text.value || !amount.value || amount.value === 0) {
    toast.error('Both fields much be filled')
    return
  }
  const transactionData = {
    text: text.value,
    amount: +amount.value
  }
  emit('submitted', transactionData)
  text.value = ''
  amount.value = 0
}
const text = ref('')
const amount = ref(0)

const emit = defineEmits(['submitted'])
</script>

<template>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input type="text" v-model="amount" id="amount" placeholder="Enter amount..." />
    </div>
    <button :class="text && (amount > 0 || amount < 0) ? 'btn darker' : 'btn'">
      Add transaction
    </button>
  </form>
</template>

<style scoped></style>
