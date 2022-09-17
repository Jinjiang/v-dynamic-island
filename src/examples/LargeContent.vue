<script setup lang="ts">
const emit = defineEmits(['click'])
</script>

<template>
  <div @click="emit('click')">
    Click to collapse
  </div>
  <div class="bottom"></div>
</template>

<style scoped>
.bottom {
  width: 100%;
  height: 60px;
  border-radius: 0 0 35px 35px;
  margin-top: 50px;
  background-color: rgba(255, 255, 255, 0.5);
}
</style>
