<template>
  <Header/>
  <Balance :total="total" />
  <IncomeExpenses :expenses="+ expenses" :income="+ income"/>
  <TransactionList :transactions="transactions" @transaction-deleted="handleTransactionDeleted"/>
  <AddTransaction @transaction-submitted="handleTransactionSubmitted"/>
  <div class="container">

  </div>
</template>

<script setup>



import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";

import { ref, computed, onMounted } from "vue";

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if(savedTransactions){
    transactions.value = savedTransactions
  }
})

const toast = useToast()

// get total
const total = computed(() => {
  return transactions.value.reduce((accumulator, transaction) => {
  return accumulator + transaction.amount;
  }, 0);
})

// get income 

const income = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount > 0)
  .reduce((accumulator, transaction) => {
    return accumulator + transaction.amount;
  }, 0).toFixed(2)
})


// get expenses 
const expenses = computed(() =>{
  return transactions.value
  .filter((transaction) => transaction.amount < 0)
  .reduce((accumulator, transaction) => {
    return accumulator + transaction.amount
  }, 0).toFixed(2)
})

// add transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(), 
    text: transactionData.text,
    amount: transactionData.amount
  })

  savedTransactionsToLocalStorage()

  toast.success('Successfully created')
}

const handleTransactionDeleted = (id) =>{
transactions.value = transactions.value.filter((transaction) => 
transaction.id !== id)

toast.success("Transaction deleted")
}

// generate unique ID
const generateUniqueId = () =>{
  return Math.floor(Math.random() * 1000000)
}

const savedTransactionsToLocalStorage = () =>{
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

</script>