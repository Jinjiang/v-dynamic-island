<script setup lang="ts">
import { ref } from 'vue';

defineProps({
  deviceWidth: {
    type: Number,
    default: Math.min(430, window.innerWidth)
  },
  left: {
    type: String,
    default: 'normal',
    validator (value: string) {
      return ['normal', 'outer', 'max'].includes(value)
    }
  },
  right: {
    type: String,
    default: 'normal',
    validator (value: string) {
      return ['normal', 'outer', 'max'].includes(value)
    }
  },
  layout: {
    type: String,
    default: 'normal',
    validator (value: string) {
      return ['normal', 'large', 'square'].includes(value)
    }
  },
  shown: Boolean,
  expanded: Boolean,
  leftResponsive: Boolean,
  rightResponsive: Boolean,
  warning: Boolean
})

const leftWidth = ref(0)
const bgLeftShown = ref(false)
const rightWidth = ref(0)
const bgRightShown = ref(false)

type TransitionEventHandler = (el: HTMLElement, done: () => void) => void

const initLeftSize: TransitionEventHandler = (el, done) => {
  const { width } = el.getBoundingClientRect()
  leftWidth.value = width
  bgLeftShown.value = true
  done()
}
const initRightSize: TransitionEventHandler = (el, done) => {
  const { width } = el.getBoundingClientRect()
  rightWidth.value = width
  bgRightShown.value = true
  done()
}
const resetLeftSize: TransitionEventHandler = (_, done) => {
  leftWidth.value = 0
  bgLeftShown.value = false
  done()
}
const resetRightSize: TransitionEventHandler = (_, done) => {
  rightWidth.value = 0
  bgRightShown.value = false
  done()
}
</script>

<template>
  <div class="container" :class="[
    `layout-left-${left}`,
    `layout-right-${right}`,
    `layout-main-${layout}`,
    {
      'layout-left-responsive': leftResponsive,
      'layout-right-responsive': rightResponsive,
      shown, warning, expanded
    }
  ]" :style="{
    '--left-width': leftWidth,
    '--right-width': rightWidth,
    '--device-width': deviceWidth
  }">
    <div class="forbidden"></div>
    <div class="content">
      <Transition name="left" @enter="initLeftSize" @leave="resetLeftSize">
        <div v-if="shown" class="left slot transition"><slot name="left" /></div>
      </Transition>
      <Transition name="bg-left">
        <div v-if="shown && bgLeftShown" class="bg-left transition"></div>
      </Transition>
      <Transition name="right" @enter="initRightSize" @leave="resetRightSize">
        <div v-if="shown" class="right slot transition"><slot name="right" /></div>
      </Transition>
      <Transition name="bg-right">
        <div v-if="shown && bgRightShown" class="bg-right transition"></div>
      </Transition>
      <Transition name="main">
        <div v-if="shown && expanded" class="main slot transition">
          <div class="main-left slot"><slot name="expanded-left" /></div>
          <div class="main-right slot"><slot name="expanded-right" /></div>
          <slot name="expanded" />
        </div>
      </Transition>
      <Transition name="bg-main">
        <div v-if="shown && expanded" class="bg-main transition"></div>
      </Transition>
    </div>
  </div>
</template>

<style scoped src="./DynamicIsland.css"></style>
