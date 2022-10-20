<template>
  <h1>{{ displayTerm }}</h1>
</template>

<script setup lang="ts">
import { ref, watchEffect } from 'vue'

const props = defineProps<{ searchTerm: string }>()

const displayTerm = ref('')
watchEffect(() => {
  displayTerm.value = props.searchTerm;
});

</script>

<style scoped>
</style>
