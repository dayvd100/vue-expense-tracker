<template>
  <Header/>
  <Balance :total="total" />
  <IncomeExpenses :expenses="+ expenses" :income="+ income"/>
  <TransactionList :transactions="transactions" :deleteTransaction="deleteTransaction" @transaction-deleted="handleTransactionDeleted"/>
  <AddTransaction :sendJson="sendJson" @transaction-submitted="handleTransactionSubmitted"/>
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
import { ref, computed} from "vue";
import axios from "axios";

const transactions = ref([]);
const toast = useToast()

// Função para deletar uma transação
const deleteTransaction = async (id) => {
  try {
    await axios.delete(`http://127.0.0.1:8000/transactions/${id}`);
    transactions.value = transactions.value.filter(transaction => transaction.id !== id);
    console.log(`Transação com ID ${id} deletada com sucesso.`);
  } catch (error) {
    console.error(`Erro ao deletar transação com ID ${id}:`, error);
  }
};

const dataApi = async () => {
    try {
      const response = await axios.get("http://127.0.0.1:8000/transactions");
      if(response.status == 200){
        transactions.value = response.data; 
        console.log(response);
      }
    } catch (error) {
      console.log(error);
    }
  }
  
  dataApi();
  

const sendJson = () => {
  try {
    const datas = {"amount_transaction": amount.value, "name_transaction": text.value,};
    axios.post("http://127.0.0.1:8000/transaction", datas);
    toast.success("Created Successfully")
  } catch (error) {
    console.log(error);
  }
}

// get total
const total = computed(() => {
  return transactions.value.reduce((accumulator, transaction) => {
  return accumulator + transaction.amount_transaction;
  }, 0);
})

// get income 
const income = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount_transaction > 0)
  .reduce((accumulator, transaction) => {
    return accumulator + transaction.amount_transaction;
  }, 0).toFixed(2)
})


// get expenses 
const expenses = computed(() =>{
  return transactions.value
  .filter((transaction) => transaction.amount_transaction < 0)
  .reduce((accumulator, transaction) => {
    return accumulator + transaction.amount_transaction
  }, 0).toFixed(2)
})

// add transaction
const handleTransactionSubmitted = () => {
  toast.success('Successfully created')
}

const handleTransactionDeleted = (id) =>{
transactions.value = transactions.value.filter((transaction) => 
transaction.id !== id)

toast.success("Transaction deleted")
}

</script>