<script setup>
const props =defineProps({
  fillerList:{
    typr: Array,
    required: true,
  }
 });
   
const emit = defineEmits(['transactionDeleted']);

const deleteTransaction = (id) => {
 emit('transactionDeleted', id)
}
</script>
<template>
    <ul  class="list">
      <span v-for="item in fillerList" :key="fillerList.id">
    <li class="ListItem" >
        <input class="checkbox" type="checkbox">
         {{ item.value }} 
         <button @click="deleteTransaction(item.id)" class="delete-btn"><i class="pi pi-times-circle"></i></button>
    </li>
      </span>
    </ul>
</template>