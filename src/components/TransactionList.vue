<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount_transaction < 0 ? 'minus' : 'plus'">
      {{ transaction.name_transaction }} <span>R$ {{ transaction.amount_transaction }}</span>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
    </li>
  </ul>
</template>

<script setup>
const props = defineProps({
  deleteTransaction:{
    type: Function,
    required: true
  },
  transactions:{
    type: Array,
    required: true
  }
})
</script>