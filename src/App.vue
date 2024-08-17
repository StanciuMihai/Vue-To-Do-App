<script setup>
  import Header from './Header.vue';
  import ListElements from './ListElements.vue';
  import AddItem from './AddItem.vue';
  import {ref,computed, onMounted} from 'vue';
  import { useToast } from 'vue-toastification';

  const toast=useToast();

  const fillerList = ref([{
        id:1,
        value:'Remember to buy soap',
        isChecked: false
    },
    {
        id:2,
        value:'Do the laundry on Tuesday',
        isChecked: false
    },
    {
        id:3,
        value:'Do not forget to exercise!',
        isChecked: false
    },
    ]);
    onMounted(()=>{
  const savedTransactions=JSON.parse(localStorage.getItem('ToDoAppData'));

  if(savedTransactions){
    fillerList.value = savedTransactions;
  }
});
    const handleTransactionSubmitted = (transactionData) => {
    
    fillerList.value.push({
      id: generateUniqueId(),
      value: transactionData.text,
      isChecked: false
    });

    saveTransactionsToLocalStorage();

    toast.success('To do added');
  };

  const handleTransactionDeleted = (id) => {
    fillerList.value=fillerList.value.filter((transaction)=>
    transaction.id !==id)

    saveTransactionsToLocalStorage();

    toast.success('To do deleted');
  };

  const handleTransactionModified = (id,value,isChecked) => {
    const index = fillerList.value.findIndex((transaction) => transaction.id === id);

    // If the transaction is found, update its properties
    if (index !== -1) {
      fillerList.value[index].value = value;
      fillerList.value[index].isChecked = isChecked;
}

    saveTransactionsToLocalStorage();

    toast.success('To do updated');
  };

  const generateUniqueId =() => {
    return Math.floor(Math.random()* 1000000)
  }
  const saveTransactionsToLocalStorage=()=>{
    localStorage.setItem('ToDoAppData',JSON.stringify(fillerList.value));
  }
</script>

<template>
  <Header />
  <AddItem @transaction-submitted="handleTransactionSubmitted"/>
  <ListElements :fillerList="fillerList"  @transaction-deleted="handleTransactionDeleted" @transactionModified="handleTransactionModified"/>
</template>

<style scoped>

</style>
