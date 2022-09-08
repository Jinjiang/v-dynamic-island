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
}

.content {
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 135px;
  height: 40px;
  font-size: 16px;
  line-height: 40px;
  background-color: black;
  border-radius: 20px;
  color: white;
  gap: 0.5em;
  padding: 0 0.5em;
  box-sizing: border-box;
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
