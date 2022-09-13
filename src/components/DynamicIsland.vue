<script setup lang="ts">import { nextTick, ref } from 'vue';

defineProps({
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

// constant:
// - large rect, normal rect, square rect
// - forbidden rect
// - max side width
// - radius, expanded radius
// state
// - left rect, right rect
// - animating?
// - blobbing?
// computed: scaleX, scaleY, initWidth, initHeight, offsetLeft, offsetRight

// transition:
// - left/right
//   show: get rect, get offset, _set rect to bg_, animate offset to fg/bg
//     normal/max: translate(bounce) + fg blur + fg opacity
//     outer: translate(bounce) + bg scale(bounce) + bg blob + fg blur + fg opacity
//   hide: animate offset to fg/bg, _reset bg rect_
//     translate + fg blur + fg opacity
//   expand/collapse: get dest rect, get dest position, animate to fg, hide bg, (+ set and animate main fg/bg)
//     _add/remove before/after_
//     responsive ? nothing : blur + opacity
// - main
//   expand: get current rect/scale, _set scale to fg, set rect to bg_, animate scale to fg, animate rect to bg
//     fg: blur, opacity, scale(bounce)
//     bg: width(bounce), height(bounce), radius
//   collapse: get rect/scale, animate scale to fg, animate rect to bg, _remove fg/bg_
//     fg: blur, opacity
//     bg: width(bounce), height, radius

const leftWidth = ref(0)
const bgLeftShown = ref(false)
const rightWidth = ref(0)
const bgRightShown = ref(false)

type TransitionInit = (el: HTMLElement, done: () => void) => void

const initLeftSize: TransitionInit = (el, done) => {
  // get width
  const { width } = el.getBoundingClientRect()
  // init and show bg-left
  leftWidth.value = width
  bgLeftShown.value = true
  done()
}
const initRightSize: TransitionInit = (el, done) => {
  // get width
  const { width } = el.getBoundingClientRect()
  // init and show bg-right
  rightWidth.value = width
  bgRightShown.value = true
  done()
}
</script>

<template>
  <div class="container" :class="[
    `layout-left-${left}`,
    `layout-left-${right}`,
    `layout-main-${layout}`,
    { 'layout-left-responsive': leftResponsive },
    { 'layout-right-responsive': rightResponsive },
  ]">
    <div class="forbidden"></div>
    <div class="content" :class="{ shown, warning, expanded }" :style="{
      '--left-offset': `${-leftWidth}px`,
      '--right-offset': `${-rightWidth}px`
    }">
      <Transition name="left" @enter="initLeftSize">
        <div v-if="shown" class="left"><slot name="left" /></div>
      </Transition>
      <Transition name="bg-left">
        <div v-if="shown && bgLeftShown" class="bg-left" :style="{
          width: `${leftWidth}px`
        }"></div>
      </Transition>
      <Transition name="right" @enter="initRightSize">
        <div v-if="shown" class="right"><slot name="right" /></div>
      </Transition>
      <Transition name="bg-right">
        <div v-if="shown && bgRightShown" class="bg-right" :style="{
          width: `${rightWidth}px`
        }"></div>
      </Transition>
      <div class="main">
        <div class="main-left"><slot name="expanded-left" /></div>
        <div class="main-right"><slot name="expanded-right" /></div>
        <slot name="expanded" />
      </div>
      <div class="bg-main"></div>
    </div>
  </div>
</template>

<style scoped src="./DynamicIsland.css"></style>
