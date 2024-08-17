<script setup>
import { ref } from 'vue'

const props = defineProps({
  fillerList: {
    type: Array,
    required: true,
  }
});

const emit = defineEmits(['transactionDeleted', 'transactionModified']);

// Function to delete the transaction
const deleteTransaction = (id) => {
  emit('transactionDeleted', id);
}

// Function to toggle edit mode and save changes
const modifyTransaction = (item) => {
  item.isEditing = true;
}

// Function to save changes and exit edit mode
const saveTransaction = (item) => {
  item.isEditing = false;
  emit('transactionModified', item.id, item.value,item.isChecked);
  
}

</script>

<template>
  <ul class="list">
    <li v-for="item in fillerList" :key="item.id" class="ListItem">
      <input 
        class="checkbox" 
        type="checkbox" 
        v-model="item.isChecked"
      />
      <!-- Conditionally render the span or input based on isEditing -->
      <span 
        v-if="!item.isEditing" 
        :class="{ 'span-show': true, 'span-strikethrough': item.isChecked }"
      >
        {{ item.value }}
      </span>
      <input 
        v-else 
        v-model="item.value"
        class="input-hide"
        @blur="saveTransaction(item)"
        @keyup.enter="saveTransaction(item)"
        type="text"
      />
      <div class="buttons-div">
        <button @click="modifyTransaction(item)" class="modify-btn">
          <i class="pi pi-pen-to-square"></i>
        </button>
        <button @click="deleteTransaction(item.id)" class="delete-btn">
          <i class="pi pi-times-circle"></i>
        </button>
      </div>
    </li>
  </ul>
</template>


