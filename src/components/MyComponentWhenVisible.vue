<script setup>
import { onMounted } from 'vue';

onMounted(() => {
  console.log('MyComponentWhenVisible mounted!');
});
</script>

<template>
  <h1>MyComponentWhenVisible</h1>
</template>

<style scoped></style>
