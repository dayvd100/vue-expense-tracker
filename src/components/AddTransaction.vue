<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount">Amount <br />(negative - expense, positive - income)</label>
      <input type="text" id="amount" v-model="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const text = ref('');
const amount = ref('');

const toast = useToast();

const props = defineProps({
  sendJson: {
    type: Function,
    required: true
  }
});

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Both fields must be filled');
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
  };

  // Chamando a função sendJson passada como prop
  props.sendJson(transactionData);

  // Emitindo evento para notificar o componente pai sobre a transação submetida
  emit('transaction-submitted', transactionData);

  // Limpar campos após envio
  text.value = '';
  amount.value = '';
  toast.success('Transaction added successfully');
};
</script>
