<script setup lang="ts">
import { ref } from 'vue';

import DynamicIsland from './components/DynamicIsland.vue'
import DemoIconApple from './components/DemoIconApple.vue'
import DemoIconCapture from './components/DemoIconCapture.vue'

// const width = window.innerWidth
// const height = window.innerHeight
</script>

<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <!-- {{ width }} x {{ height }} -->
  <div class="stage">
    <DynamicIsland class="island">
      <!-- <DemoIconApple :size="1.5" @click="expanded = !expanded" /> -->
      <!-- <DemoIconCapture v-if="expanded" :size="3" /> -->
    </DynamicIsland>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.stage {
  background: white url(/background.png) top center no-repeat;
  background-size: contain;
  position: relative;
  width: 487px; /* 974px */
  height: 438px; /* 876px */
}
.island {
  position: absolute;
  left: 50%;
  top: 35px; /* 71px; */
  translate: -50% 0;
}
</style>
