<script setup>
import { onMounted, defineProps, computed } from 'vue';

const props = defineProps({
  hydrated: {
    type: Boolean,
    required: true,
  }
});

onMounted(() => {
  console.log('MyComponentOnTrigger mounted!');
});

const onClick = () => {
  console.log('MyComponentOnTrigger button clicked!');
};

const buttonMessage = computed(() => {
  const isHydrated = props.hydrated === true;
  return `Click me! (${isHydrated ? 'Hydrated' : 'Not Hydrated'})`;
});
</script>

<template>
  <h1>MyComponentOnTrigger</h1>
  <button @click="onClick">{{ buttonMessage }}</button>
</template>

<style scoped></style>
