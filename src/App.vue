<script setup lang="ts">
import { computed, ref } from 'vue';

import DynamicIsland from './components/DynamicIsland.vue'
import DemoIconApple from './components/DemoIconApple.vue'
import DemoIconTimer from './components/DemoIconTimer.vue'

// import Test from './components/Test.vue'

const shown = ref(false)
const warning = ref(false)
const expanded = ref(false)

const props = computed(() => ({
  shown: shown.value,
  expanded: shown.value && expanded.value,
  warning: warning.value,
  leftResponsive: true,
  // rightResponsive: true,
  // left: 'max',
  // right: 'max',
  // layout: 'large',
}))
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
  <!-- <Test /> -->
  <p>
    <button @click="expanded = false, shown = !shown">Visible</button>
    <button @click="warning = !warning">Warn</button>
  </p>
  <div class="stage">
    <DynamicIsland class="island" v-bind="props">
      <template #left><DemoIconApple @click="expanded = true" /></template>
      <template #right><span>Ring</span></template>
      <template #expanded><span @click="expanded = false, shown = false">Hello World!</span></template>
      <!-- <template #expanded-left><DemoIconApple :size="52" /></template> -->
      <template #expanded-right><DemoIconTimer :size="52" /></template>
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
