<script setup>
import {useAuthStore} from "@/stores/auth";


const authStore = useAuthStore();
</script>

<template>
  <header>
    

    <div class="wrapper ">
      
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

