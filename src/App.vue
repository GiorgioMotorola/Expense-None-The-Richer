<template>
<Header />
<div class="container">
  <Balance :total="+total" />
  <IncomeExpenses :income="+income" :expenses="+expenses"/>
  <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionsDeleted" />
  <AddTransaction @transactionsSubmitted="handleTransactionsSubmitted"/>
</div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { useToast } from 'vue-toastification';

import { ref, computed } from 'vue';

const toast = useToast();

const transactions = ref([
        { id: 1, text: 'Flower', amount: -19.99 },
        { id: 2, text: 'Salary', amount: 299.97 },
        { id: 3, text: 'Book', amount: -10 },
        { id: 4, text: 'Camera', amount: 150 },
    ]);

    // get total
    const total = computed(() => {
      return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0);
    });

    // get income
    const income = computed(() => {
      return transactions.value
      .filter((transaction) => transaction.amount > 0)
      .reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0)
      .toFixed(2);
    });


    // get expenses
    const expenses = computed(() => {
      return transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((acc, transaction) => {
        return acc + transaction.amount;
      }, 0)
      .toFixed(2);
    });

    //Add Transaction
    const handleTransactionsSubmitted = (transactionData) => {
      transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount,
      });
      toast.success('Transaction Added!');
    };

    //Generate Unique Id
    const generateUniqueId = () => {
      return Math.floor(Math.random() * 1000000);
    };

    // Delete Transaction
    const handleTransactionsDeleted = (id) => {
      transactions.value = transactions.value.filter((transaction) => 
        transaction.id !== id
      );
      toast.success('Transaction Deleted!');
    };
</script>