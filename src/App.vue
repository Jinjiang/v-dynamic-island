<script setup lang="ts">
import { ref } from 'vue';
import GitHubCorner from './examples/GitHubCorner.vue';
import DemoA from './examples/DemoA.vue';
import DemoB from './examples/DemoB.vue';
import DemoC from './examples/DemoC.vue';
// import DynamicIsland from '../../dist/dynamic-island.js';
// import '../../dist/style.css';
// import Test from './components/Test.vue'

const shown = ref(false)
const warning = ref(false)
const expanded = ref(false)

const hasBackground = window.innerWidth > 430

const key = ref('A')
const updateDemo = () => {
  shown.value = false
  warning.value = false
  expanded.value = false
}
</script>

<template>
  <h1>Vue Dynamic Island</h1>
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
    <p><select v-model="key" @change="updateDemo">
      <option value="A" selected>Demo A</option>
      <option value="B" selected>Demo B</option>
      <option value="C" selected>Demo C</option>
    </select></p>
    <p><button @click="expanded = false, shown = !shown, warning = false">Visible</button></p>
    <p><button :disabled="!shown || warning || expanded" @click="warning = true">Warn</button></p>
  </div>
  <div class="stage" :class="{ 'has-background': hasBackground }">
    <DemoA
      v-if="key === 'A'"
      :shown="shown"
      :warning="warning"
      :expanded="expanded"
      @expand="expanded = true"
      @collapse="expanded = false" />
    <DemoB
      v-else-if="key === 'B'"
      :shown="shown"
      :warning="warning"
      :expanded="expanded"
      @expand="expanded = true"
      @collapse="expanded = false" />
    <DemoC
      v-else-if="key === 'C'"
      :shown="shown"
      :warning="warning"
      :expanded="expanded"
      @expand="expanded = true"
      @collapse="expanded = false" />
  </div>
  <p class="credits">The iPhone background image is from apple.com.</p>
  <GitHubCorner url="https://github.com/Jinjiang/vue-dynamic-island" />
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
  background: white url(./assets/background.png) top center no-repeat;
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

.credits {
  font-size: 0.8em;
  text-align: center;
  color:#42b883aa;
}
</style>
