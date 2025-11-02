<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref } from 'vue'; //make it reactive

const transactions = ref([
      { id: 1, text: 'Cash', amount: -400 },
      { id: 2, text: 'Paycheck', amount: 800 },
      { id: 3, text: 'Flower', amount: -19.99 },
      { id: 4, text: 'Camera', amount: 150.99 },
    ]);
</script>

<template>
    <Header />
    <div class="container">
        <Balance />
        <IncomeExpense />
        <TransactionList :transactions="transactions" /> <!-- pass transactions as prop -->
        <AddTransaction />
    </div>
</template>

<style scoped>
</style>
