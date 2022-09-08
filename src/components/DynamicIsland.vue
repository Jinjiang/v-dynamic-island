<script setup lang="ts">
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
    <div class="content" :class="{ shown, warning, expanded }">
      <div class="left"><slot name="left" /></div>
      <div class="bg-left"></div>
      <div class="right"><slot name="right" /></div>
      <div class="bg-right"></div>
      <div class="main">
        <div class="main-left"><slot name="expanded-left" /></div>
        <div class="main-right"><slot name="expanded-right" /></div>
        <slot name="expanded" />
      </div>
      <div class="bg-main"></div>
    </div>
  </div>
</template>

<style scoped>
.container {
  min-width: 135px;
  height: 40px;
  position: relative;
}

.forbidden {
  width: 135px;
  height: 40px;
  background-color: black;
  border-radius: 20px;
}

.content {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  color: white;
  font-size: 16px;
}

.left,
.right {
  position: absolute;
  top: 0;
  height: 40px;
  white-space: nowrap;
  padding: 0 10px; /* temp */
  box-sizing: border-box;
  display: flex;
  align-items: center;
}
.bg-left,
.bg-right {
  position: absolute;
  top: 0;
  height: 40px;
  background-color: black;
  z-index: -1;
}
.left {
  left: -100px; /* for test */
}
.bg-left {
  left: -100px; /* for test */
  width: 40px; /* for test */
  border-top-left-radius: 20px;
  border-bottom-left-radius: 20px;
}
.right {
  right: -100px; /* for test */
}
.bg-right {
  right: -100px; /* for test */
  width: 40px; /* for test */
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
}

.warning {
  animation: shake 0.1s 2;
}
@keyframes shake {
  0% {
    transform: translateX(0);
  }
  25% {
    transform: translateX(-10px);
  }
  50% {
    transform: translateX(0);
  }
  75% {
    transform: translateX(10px);
  }
  100% {
    transform: translateX(0);
  }
}
</style>
