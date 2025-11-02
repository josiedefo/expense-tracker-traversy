<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions = ref([
      { id: 1, text: 'Cash', amount: -400 },
      { id: 2, text: 'Paycheck', amount: 800 },
      { id: 3, text: 'Flower', amount: -19.99 },
      { id: 4, text: 'Camera', amount: 150.99 },
      { id: 5, text: 'Bread', amount: -5.99 },
    ]);

//Get total amount
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => acc + transaction.amount, 0);
});

//Get income
const income = computed(() => {
    return transactions.value
        .filter(transaction => transaction.amount > 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0).toFixed(2);
});

//Get expense
const expense = computed(() => {
    return transactions.value
        .filter(transaction => transaction.amount < 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0).toFixed(2);
});

//Handle new transaction submission
const handleTransactionSubmitted = (newTransaction) => {
    transactions.value.push({...newTransaction, id: generateUniqueId()});

    console.log('New transaction added:', newTransaction);
    toast.success('Transaction added successfully!');
};

const generateUniqueId = () => {
  return Date.now() + Math.floor(Math.random() * 1000);
};
</script>

<template>
    <Header />
    <div class="container">
        <Balance :total/>
        <IncomeExpense :income :expense/>
        <TransactionList :transactions="transactions" />
        <AddTransaction @add-transaction="handleTransactionSubmitted" />
    </div>
</template>

<style scoped>
</style>
