<script setup>
  import Header from './Header.vue';
  import ListElements from './ListElements.vue';
  import {ref,computed, onMounted} from 'vue';

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
</script>

<template>
  <Header />
  <ListElements :fillerList="fillerList"/>
</template>

<style scoped>

</style>
