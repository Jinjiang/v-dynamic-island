<script setup lang="ts">
import { computed, ref } from 'vue';

import DynamicIsland from './components/DynamicIsland.vue'
import DemoIconApple from './components/DemoIconApple.vue'
import DemoIconTimer from './components/DemoIconTimer.vue'

// import Test from './components/Test.vue'

const shown = ref(false)
const warning = ref(false)
const expanded = ref(false)

const hasBackground = window.innerWidth > 430

const props = computed(() => ({
  shown: shown.value,
  expanded: shown.value && expanded.value,
  warning: warning.value,
  superLeading: {},
  // superTrailing: {},
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
  <div class="controls">
    <p><button @click="expanded = false, shown = !shown, warning = false">Visible</button></p>
    <p><button :disabled="!shown || warning" @click="warning = true">Warn</button></p>
  </div>
  <div class="stage" :class="{ 'has-background': hasBackground }">
    <DynamicIsland class="island" v-bind="props">
      <template #leading><DemoIconApple @click="expanded = true" /></template>
      <template #trailing><span>Ring</span></template>
      <template #expanded><span @click="expanded = false, shown = false">Hello World!</span></template>
      <!-- <template #expanded-leading><DemoIconApple :size="52" /></template> -->
      <template #expanded-trailing><DemoIconTimer :size="52" /></template>
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
  position: relative;
  width: 100%;
  height: 438px;
}
.stage.has-background {
  background: white url(/background.png) top center no-repeat;
  background-size: contain;
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
