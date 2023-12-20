<template>
    
    <h3>History</h3>
      <ul id="list" class="list">
        <li v-for="transaction in transactions" :key='transaction.id' :class="transaction.amount < 0 ?'minus':'plus'" :id='transaction.id' >
        {{transaction.text}} <span>{{transaction.amount}}</span><button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
        </li>
      
      </ul>
</template>
<script setup>

const emit = defineEmits('deleteTransaction');
const deleteTransaction =(id)=>{
  // console.log(e.target.parentElement)

  emit('deleteTransaction',id)
}
 const props = defineProps({
  transactions: { 
    type:Array,
  required:true
},
 });

</script>