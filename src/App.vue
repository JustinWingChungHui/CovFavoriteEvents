<template>
  <GoogleImageLoader :searchTerm="searchTerm" />
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import GoogleImageLoader from './components/GoogleImageLoader.vue'

const data = ref(new Array<string>())
const searchTerm = ref("Brian May")

onMounted(async () => {
  const response = await fetch('/events2020.txt');
  const text = await response.text()

  data.value = text.split('\n')

  window.setInterval(setRandomSearchTerm, 3000);
});

const setRandomSearchTerm = () => {  
  const index = Math.floor(Math.random() * data.value.length);
  searchTerm.value = data.value[index];
};

</script>

<style scoped>
</style>
