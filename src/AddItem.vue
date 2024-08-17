<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const text= ref('');


const emit = defineEmits(['transactionSubmitted']);

const toast=useToast();

  const onSubmit = () => {

    if(!text.value){
      toast.error('Please enter something to do before submitting');
      return;
    }

    
   
    
    const transactionData={
      text:text.value
    }
    emit('transactionSubmitted',transactionData);
   
    text.value='';

  }
</script>

<template>
     <form id="form" @submit.prevent="onSubmit">
        <input type="text" id="text" v-model="text" placeholder="Add something to do..." />
        <button class="btn">Add</button>
     </form>
</template>