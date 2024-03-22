<script setup lang="ts">
interface Props {
  total: number
}
const props = defineProps<Props>()
</script>

<template>
  <h4>Your Balance</h4>
  <h1 id="balance">${{ props.total }}</h1>
</template>

<style scoped></style>
