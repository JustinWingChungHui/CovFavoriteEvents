<template>
  <GoogleImageLoader :event-info="eventInfo" :image-url="imageUrl" @image-loaded="imageLoaded"/>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { titleCase } from "title-case";
import GoogleImageLoader from './components/GoogleImageLoader.vue'

const data = ref(new Array<string>())
const eventInfo = ref("")
const imageUrl = ref("");

onMounted(async () => {
  const response = await fetch('/events2020.txt');
  const text = await response.text()

  data.value = text.split('\n')
  setRandomSearchTerm();
});

const imageLoaded = () => {
  window.setTimeout(setRandomSearchTerm, 5000);
};

const setRandomSearchTerm = () => {  
  const index = Math.floor(Math.random() * data.value.length);
  const entry = data.value[index].split('|');

  if (entry.length === 2) {
    eventInfo.value = titleCase(entry[0]);
    imageUrl.value = titleCase(entry[1]);
  }
};

</script>

<style scoped>
</style>
