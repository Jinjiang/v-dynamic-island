<script setup lang="ts">
import { ref } from 'vue';

type Rect = {
  top?: number
  left?: number
  right?: number
  size?: number
}

// TODO: export
type Props = {
  deviceWidth?: number
  shown?: boolean
  expanded?: boolean
  warning?: boolean
  superLeading?: Rect
  superTrailing?: Rect
}

withDefaults(defineProps<Props>(), {
  deviceWidth: Math.min(430, window.innerWidth)
})

const leadingWidth = ref(0)
const leadingBgShown = ref(false)
const trailingWidth = ref(0)
const trailingBgShown = ref(false)

type TransitionEventHandler = (el: HTMLElement, done: () => void) => void

const initLeadingSize: TransitionEventHandler = (el, done) => {
  const { width } = el.getBoundingClientRect()
  leadingWidth.value = width
  leadingBgShown.value = true
  done()
}
const initTrailingSize: TransitionEventHandler = (el, done) => {
  const { width } = el.getBoundingClientRect()
  trailingWidth.value = width
  trailingBgShown.value = true
  done()
}
const resetLeadingSize: TransitionEventHandler = (_, done) => {
  leadingWidth.value = 0
  leadingBgShown.value = false
  done()
}
const resetTrailingSize: TransitionEventHandler = (_, done) => {
  trailingWidth.value = 0
  trailingBgShown.value = false
  done()
}

// TODO: calculate expanded height
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
    '--device-width': deviceWidth,
    '--expanded-leading-size': superLeading?.size,
    '--expanded-leading-top': superLeading?.top,
    '--expanded-leading-left': superLeading?.left,
    '--expanded-trailing-size': superTrailing?.size,
    '--expanded-trailing-top': superTrailing?.top,
    '--expanded-trailing-right': superTrailing?.right,
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
      <Transition name="main">
        <div v-if="shown && expanded" class="main slot transition">
          <div class="main-leading slot"><slot name="expanded-leading" /></div>
          <div class="main-trailing slot"><slot name="expanded-trailing" /></div>
          <slot name="expanded" />
        </div>
      </Transition>
      <Transition name="main-bg">
        <div v-if="shown && expanded" class="main-bg transition"></div>
      </Transition>
    </div>
  </div>
</template>

<style scoped src="./DynamicIsland.css"></style>
