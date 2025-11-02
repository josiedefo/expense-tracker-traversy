<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
    // Load transactions from localStorage if available
    const savedTransactions = localStorage.getItem('transactions');
    if (savedTransactions) {
        transactions.value = JSON.parse(savedTransactions);
    }
});

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
    saveTransactionsToLocalStorage();
    console.log('New transaction added:', newTransaction);
    toast.success('Transaction added successfully!');
};

const generateUniqueId = () => {
  return Date.now() + Math.floor(Math.random() * 1000);
};

//Handle transaction deletion
const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(transaction => transaction.id !== id);
    saveTransactionsToLocalStorage();
    toast.success('Transaction deleted successfully.');
};

//Watch transactions and save to localStorage
const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
};
</script>

<template>
    <Header />
    <div class="container">
        <Balance :total/>
        <IncomeExpense :income :expense/>
        <TransactionList :transactions="transactions" @delete-transaction="handleTransactionDeleted" />
        <AddTransaction @add-transaction="handleTransactionSubmitted" />
    </div>
</template>

<style scoped>
</style>
