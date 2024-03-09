<script setup>
import { ref, computed, onMounted } from 'vue';
import AddTransaction from './components/AddTransaction.vue';
import Balance from './components/Balance.vue';
import Header from './components/Header.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import Transactions from './components/Transactions.vue';
import { useToast } from 'vue-toastification';
const toast = useToast();
const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if (savedTransactions && savedTransactions.length > 0) {
    transactions.value = savedTransactions;
  }
})

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}

const total = computed(() => {
  return transactions.value
    .reduce((acc, transaction) => (acc += transaction.amount), 0);
});

const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => (acc += transaction.amount), 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => (acc += transaction.amount), 0);
});

const handleAddTrasaction = (newTransaction) => {
  transactions.value = [...transactions.value, newTransaction];
  saveTransactionsToLocalStorage();
  toast.success('Transaction added successfully!');
}

const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
  saveTransactionsToLocalStorage();
  toast.error('Transaction deleted successfully!');
}

</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :expense="expense" :income="income" />
    <Transactions :transactions="transactions" @delete-transaction="handleDeleteTransaction" />
    <AddTransaction @add-transaction="handleAddTrasaction" />
  </div>
</template>
