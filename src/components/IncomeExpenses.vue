<script setup lang="ts">
interface Props {
  income: number
  expense: number
}
const props = defineProps<Props>()
</script>

<template>
  <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <p id="money-plus" class="money plus">+${{ props.income }}</p>
    </div>
    <div>
      <h4>Expense</h4>
      <p id="money-minus" class="money minus">-${{ props.expense }}</p>
    </div>
  </div>
</template>

<style scoped></style>
