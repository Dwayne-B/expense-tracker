<template>
  <HeaderSection/>
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpense :income="+income" :expense="+expense"/>
    <TransactionList @deleteTransaction="handleDeleteTransaction"
    :transactions="transactions"/>
    <AddTransactions @transactionSubmit = 'handleTransactioSubmit'/>
  </div>
</template>
<script setup>

import HeaderSection from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransactions from './components/AddTransaction.vue';
import {ref,computed,onMounted} from 'vue';
import {useToast } from 'vue-toastification';
const toast = useToast();



// fetch from local storage
onMounted(()=>{
  const savedData = JSON.parse(localStorage.getItem('transactions'));
  if (savedData)
    transactions.value  =savedData;
})
const transactions=ref(

[
  
]
)

const total= computed(()=>{
return transactions.value.reduce((acc,transaction)=>{
  return acc + transaction.amount ;},0).toFixed(2);
})

// get Income
const income= computed(()=>{
return transactions.value.filter((transaction)=>
  transaction.amount > 0
).reduce((acc,transaction)=>{
  console.log(transaction)
  return acc + transaction.amount; },0).toFixed(2)
})
// get Expenses
const expense= computed(()=>{
return transactions.value.filter((transaction)=>
  transaction.amount < 0
).reduce((acc,transaction)=>{
 
  return acc + transaction.amount;},0).toFixed(2)
})

// add transaction

const handleTransactioSubmit =(transactionData)=>{ transactions.value.push({
  id:genUniqueId(),
  text:transactionData.text
, amount:transactionData.amount})

saveToLocalStorage();

}


const genUniqueId = ()=>{
 return Math.random(); 
}
// delete transaction
const handleDeleteTransaction = (id)=>{
  transactions.value = transactions.value.filter((transaction)=>transaction.id !== id);
  toast.success("Item successfully deleted");
console.log(id)

saveToLocalStorage();}

const saveToLocalStorage =()=>{
  localStorage.setItem('transactions',JSON.stringify(transactions.value));
  
  
  };
</script>