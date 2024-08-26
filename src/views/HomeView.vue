<script setup>
import Counter from "@/components/Counter.vue";
import { useCounterStore } from "@/stores/counter";



const store = useCounterStore();
console.log(store.count);
</script>

<template>
  <main>
   Home Page
    
   <Counter/>
  </main>
</template>
