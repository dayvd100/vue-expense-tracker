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
  import axios from 'axios';
  import { ref } from 'vue';
  
  const transactions = ref([]);
  
  const dataApi = async () => {
    try {
      const response = await axios.get("http://127.0.0.1:8000/transactions");
      transactions.value = response.data; 
      console.log(response);
    } catch (error) {
      console.log(error);
    }
  }
  
  dataApi();
  
  const emit = defineEmits(['transactionDeleted']);
  
  const props = defineProps({
    transactions: {
      type: Array,
      required: true
    }
  });
  
  const deleteTransaction = (id) => {
    emit('transactionDeleted', id);
  }
  </script>
  