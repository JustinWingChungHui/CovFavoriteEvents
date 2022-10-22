<template>
  <h1 class="question">What Cultural Events did you attend in 2020?</h1>
  <div class="background" :style="{ backgroundImage: backgroundUrl }">
  </div>
  <h1 class="answer">{{ displayTerm }}</h1>
</template>

<script setup lang="ts">
import { defineEmits, ref, watch } from 'vue'

const props = defineProps<{ 
  eventInfo: string,
  imageUrl: string 
}>();

const backgroundUrl = ref('');
const displayTerm = ref('')
const imageData = ref<string>('');

const emit = defineEmits(['imageLoaded']);

watch(() => props.imageUrl, async () => {

  try {
    console.log(`downloading ${props.imageUrl}`)
    // Test image exists
    imageData.value = await imageUrlToBase64(props.imageUrl);
    console.log(imageData.value);
    backgroundUrl.value = `url('${props.imageUrl}')`;
    displayTerm.value = props.eventInfo;
  } finally {
    emit('imageLoaded');
  }
});


const imageUrlToBase64 = async (url: string): Promise<string> => {
  const response = await fetch(url, {
    mode: 'no-cors'
  });
  const blob = await response.blob();
  return new Promise((onSuccess, onError) => {
    try {
      const reader = new FileReader() ;
      reader.onload = function(){ onSuccess(this.result as string) } ;
      reader.readAsDataURL(blob) ;
    } catch(e) {
      onError(e);
    }
  });
};

</script>

<style scoped>
.background {
  width: 100%;
  height: 80%;
  position: absolute; 
  top: 3em; 
  left: 0;
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center;
}

.answer {
  bottom: 0%;
  position: fixed;
  width: 100%;
  margin: 0;
  padding-top: 1em;
  padding-bottom: 1em;
  text-align: center;
}

.question {
  top: 0%;
  position: fixed;
  z-index: 10;
  background-color: #242424;
  width: 100%;
  margin: 0;
  padding-top: 1em;
  padding-bottom: 1em;
  text-align: center;
}
</style>
