<template>
  <div class="background" :style="{ backgroundImage: backgroundUrl }">
    <h1>{{ displayTerm }}</h1>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from 'vue'

const props = defineProps<{ 
  eventInfo: string,
  imageUrl: string 
}>();

const backgroundUrl = ref('');
const displayTerm = ref('')

watchEffect(() => {
  backgroundUrl.value = `url('${props.imageUrl}')`;
  displayTerm.value = props.eventInfo;
});

</script>

<style scoped>
.background {
  width: 100%;
  height: 100%;
  position: absolute; 
  top: 0; 
  left: 0;
  background-repeat: no-repeat;
  background-size: contain;
}
</style>
