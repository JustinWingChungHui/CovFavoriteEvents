<template>
  <h1 class="question">What Cultural Events did you attend in 2020?</h1>
  <transition name="slide-fade">
    <img class="background" :key="props.imageUrl" :src="props.imageUrl" @error="onError"/>
  </transition>
  <h1 class="answer">{{ props.eventInfo }}</h1>
</template>

<script setup lang="ts">
import { defineEmits } from 'vue'

const props = defineProps<{ 
  eventInfo: string,
  imageUrl: string 
}>();

const emit = defineEmits(['loadFailed']);

const onError = ()=> {
  console.log('onError');
  emit('loadFailed');
};
</script>

<style scoped>
.background {
  height: 80vh;
  position: absolute; 
  max-width: 100vw;
  object-fit: contain;
  top: 8em; 
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  text-align: center;
}

.answer {
  bottom: 0%;
  position: fixed;
  width: 100vw;
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
  width: 100vw;
  margin: 0;
  padding-top: 1em;
  padding-bottom: 1em;
  text-align: center;
}

/*
  Enter and leave animations can use different
  durations and timing functions.
*/
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}
</style>
