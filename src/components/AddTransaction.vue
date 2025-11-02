<template>
    <h3>Add new transaction</h3>
      <form id="form" @submit.prevent="addTransaction">
        <div class="form-control">
          <label for="text">Text</label>
          <input type="text" id="text" placeholder="Enter text..." v-model="text" />
        </div>
        <div class="form-control">
          <label for="amount"
            >Amount <br />
            (negative - expense, positive - income)</label
          >
          <input type="number" id="amount" v-model="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
      </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const text = ref('');
const amount = ref(0);

const emit = defineEmits(['add-transaction']);

const toast = useToast();

const addTransaction = () => {
  if (text.value.trim() === '' || amount.value === 0) {
    toast.error('Please enter a valid transaction.');
    return;
  }

  const newTransaction = {
    text: text.value,
    amount: Number(amount.value)
  };

  // Emit the new transaction to the parent component
  emit('add-transaction', newTransaction);

  // Reset the form
  text.value = '';
  amount.value = 0;
};
</script>
