<template>
  <div class="spinner-border" role="status"></div>
  <p>Loading...</p>
</template>

<script setup>
defineProps({
  text: {
    type: String,
  },
});
</script>
