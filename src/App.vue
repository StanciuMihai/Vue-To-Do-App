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

  const handleTransactionMovedDown = (id) => {
  const index = fillerList.value.findIndex((transaction) => transaction.id === id);

  // Check if the item is not the last one in the array
  if (index < fillerList.value.length - 1) {
    // Swap the current item with the item to its right
    [fillerList.value[index], fillerList.value[index + 1]] = [fillerList.value[index + 1], fillerList.value[index]];

    saveTransactionsToLocalStorage();

    toast.success('To do moved down');
  } else {
    toast.error('Cannot move down. Already at the bottom.');
  }
};

  const handleTransactionMovedUp = (id) => {
  const index = fillerList.value.findIndex((transaction) => transaction.id === id);

  // Ensure the item is not the first one in the array
  if (index > 0) {
    // Swap the current item with the item to its left
    [fillerList.value[index - 1], fillerList.value[index]] = [fillerList.value[index], fillerList.value[index - 1]];

    saveTransactionsToLocalStorage();

    toast.success('To do moved up');
  } else {
    toast.error('Cannot move up. Already at the top.');
  }
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
  <ListElements :fillerList="fillerList"  
                @transaction-deleted="handleTransactionDeleted" 
                @transactionModified="handleTransactionModified"
                @transactionMovedDown="handleTransactionMovedDown"
                @transactionMovedUp="handleTransactionMovedUp"/>
</template>

<style scoped>

</style>
