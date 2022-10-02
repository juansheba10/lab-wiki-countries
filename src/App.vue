<script setup>
import Navbar from "./components/Navbar.vue";
import { onMounted } from "vue";
</script>

<template>
  <div class="app">
    <Navbar />
    <RouterView />
  </div>
</template>

<style scoped></style>
