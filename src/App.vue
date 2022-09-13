<script setup lang="ts">
import { ref } from 'vue';

import DynamicIsland from './components/DynamicIsland.vue'
import DemoIconApple from './components/DemoIconApple.vue'
import DemoIconCapture from './components/DemoIconCapture.vue'

const shown = ref(false)
const warning = ref(false)
const expanded = ref(false)
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
  <p>
    <button @click="shown = !shown">Visible</button>
    <button @click="warning = !warning">Warn</button>
  </p>
  <div class="stage">
    <DynamicIsland
      class="island"
      :shown="shown"
      :warning="warning"
      :expanded="expanded"
      left-responsive
    >
      <template #left><DemoIconApple @click="expanded = true" /></template>
      <template #right><span>Alerm</span></template>
      <template #expanded><span @click="expanded = false, shown = false">Hello World!</span></template>
      <template #expanded-right><DemoIconCapture :size="52" /></template>
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
  width: 487px;
  height: 438px;
}
.island {
  position: absolute;
  left: 50%;
  top: 35px;
  translate: -50% 0;
}
</style>
