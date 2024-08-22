<script setup>
import { onMounted } from 'vue';

onMounted(() => {
  console.log('MyComponentWhenIdle mounted!');
});
</script>

<template>
  <h1>MyComponentWhenIdle</h1>
</template>

<style scoped></style>
