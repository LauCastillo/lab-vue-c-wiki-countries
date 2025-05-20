<script setup>
import Navbar from './components/Navbar.vue'
</script>

<template>
  <div class="app">
  <Navbar />
  </div>
    <div class="container">
      <div class="row">
        <router-view />
      </div>
    </div>
</template>



<style scoped> 
</style>