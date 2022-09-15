<script lang="ts">
export type Rect = {
  top?: number
  left?: number
  right?: number
  size?: number
}

export type Props = {
  deviceWidth?: number
  shown?: boolean
  expanded?: boolean
  warning?: boolean
  superLeading?: boolean | Rect
  superTrailing?: boolean | Rect
}
</script>

<script setup lang="ts">
import { ref } from 'vue';

withDefaults(defineProps<Props>(), {
  deviceWidth: Math.min(430, window.innerWidth)
})

const getRectProp = (rect: boolean | Rect | undefined, key: keyof Rect) => {
  if (!rect || rect === true) {
    return undefined
  }
  return rect[key]
}

const leadingWidth = ref(0)
const leadingBgShown = ref(false)

type TransitionEventHandler = (el: HTMLElement, done: () => void) => void

const initLeadingSize: TransitionEventHandler = (el, done) => {
  const { width } = el.getBoundingClientRect()
  leadingWidth.value = width
  leadingBgShown.value = true
  done()
}
const resetLeadingSize: TransitionEventHandler = (_, done) => {
  leadingWidth.value = 0
  leadingBgShown.value = false
  done()
}

const trailingWidth = ref(0)
const trailingBgShown = ref(false)

const initTrailingSize: TransitionEventHandler = (el, done) => {
  const { width } = el.getBoundingClientRect()
  trailingWidth.value = width
  trailingBgShown.value = true
  done()
}
const resetTrailingSize: TransitionEventHandler = (_, done) => {
  trailingWidth.value = 0
  trailingBgShown.value = false
  done()
}

const DEFAULT_EXPANDED_HEIGHT = 92

const mainHeight = ref(DEFAULT_EXPANDED_HEIGHT)
const mainShown = ref(false)

const initMainSize: TransitionEventHandler = (el, done) => {
  const { height } = el.getBoundingClientRect()
  mainHeight.value = height
  mainShown.value = true
  done()
}
const resetMainSize: TransitionEventHandler = (_, done) => {
  mainHeight.value = DEFAULT_EXPANDED_HEIGHT
  mainShown.value = false
  done()
}
</script>

<template>
  <div class="container" :class="[
    {
      'super-leading': superLeading,
      'super-trailing': superTrailing,
      shown, warning, expanded
    }
  ]" :style="{
    '--leading-width': leadingWidth,
    '--trailing-width': trailingWidth,
    '--expanded-height': mainHeight,
    '--device-width': deviceWidth,
    '--expanded-leading-size': getRectProp(superLeading, 'size'),
    '--expanded-leading-top': getRectProp(superLeading, 'top'),
    '--expanded-leading-left': getRectProp(superLeading, 'left'),
    '--expanded-trailing-size': getRectProp(superTrailing, 'size'),
    '--expanded-trailing-top': getRectProp(superTrailing, 'top'),
    '--expanded-trailing-right': getRectProp(superTrailing, 'right'),
  }">
    <div class="forbidden"></div>
    <div class="content">
      <Transition name="leading" @enter="initLeadingSize" @leave="resetLeadingSize">
        <div v-if="shown" class="leading slot transition"><slot name="leading" /></div>
      </Transition>
      <Transition name="leading-bg">
        <div v-if="shown && leadingBgShown" class="leading-bg transition"></div>
      </Transition>
      <Transition name="trailing" @enter="initTrailingSize" @leave="resetTrailingSize">
        <div v-if="shown" class="trailing slot transition"><slot name="trailing" /></div>
      </Transition>
      <Transition name="trailing-bg">
        <div v-if="shown && trailingBgShown" class="trailing-bg transition"></div>
      </Transition>
      <Transition name="main" @enter="initMainSize" @leave="resetMainSize">
        <div
          v-if="shown && expanded"
          class="main slot transition"
          :class="{ 'active-transition': mainShown }"
        >
          <div class="main-leading slot"><slot name="expanded-leading" /></div>
          <div class="main-trailing slot"><slot name="expanded-trailing" /></div>
          <slot name="expanded" />
        </div>
      </Transition>
      <Transition name="main-bg">
        <div
          v-if="shown && expanded"
          class="main-bg transition"
        ></div>
      </Transition>
    </div>
  </div>
</template>

<style scoped src="./DynamicIsland.css"></style>
