<script setup>
const props =defineProps({
  fillerList:{
    typr: Array,
    required: true,
  }
 });
   
</script>
<template>
    <div class="ListItem" v-for="item in fillerList">
        <input class="checkbox" type="checkbox"> {{ item.value }} 
    </div>
</template>