<template>
  <GoogleImageLoader :event-info="eventInfo" :image-url="imageUrl" @load-failed="setRandomAiImage"/>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { titleCase } from "title-case";
import GoogleImageLoader from './components/GoogleImageLoader.vue'

const data = ref(new Array<string>())
const eventInfo = ref("")
const imageUrl = ref("");
const useGoogle = ref(false);
const slideTimeMs = 10000;

onMounted(async () => {
  if (window.location.href.toLocaleLowerCase().includes('google')) {
    useGoogle.value = true;
    await startGoogleImageLoader();
  } else {
    await startAiImageLoader();
  }
});

const startGoogleImageLoader = async () => {
  const response = await fetch('/events2020_googleimages.txt');
  const text = await response.text()

  data.value = text.split('\n')
  setRandomGoogleImage();
  window.setInterval(setRandomGoogleImage, slideTimeMs);
};

const setRandomGoogleImage = () => {  
  const index = Math.floor(Math.random() * data.value.length);
  const entry = data.value[index].split('|');

  if (entry.length === 2) {
    eventInfo.value = titleCase(entry[0]);
    imageUrl.value = entry[1];
  }
};


const startAiImageLoader = async () => {
  const response = await fetch('/events2020.txt');
  const text = await response.text()
  data.value = text.split('\n')

  setRandomAiImage();
  window.setInterval(setRandomAiImage, slideTimeMs);
};

const setRandomAiImage = () => {  
  const index = Math.floor(Math.random() * data.value.length);
  console.log('data.value', data.value);
  console.log('index', index);
  const entry = data.value[index];

  console.log('entry', entry);
  const image = `/ai-images/${entry}.jpg`;

  console.log('image', image);

  eventInfo.value = titleCase(entry);
  imageUrl.value = image;  
};

const onLoadFailed = () => {
  if (useGoogle.value) {
    setRandomGoogleImage();
  } else {
    setRandomAiImage();
  }
};
</script>

<style scoped>
</style>
