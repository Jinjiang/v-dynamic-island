<script setup lang="ts">
import DynamicIsland from '../components/DynamicIsland.vue';
import IconApple from './IconApple.vue'
import IconTimer from './IconTimer.vue'

export type Props = {
  shown?: boolean
  warning?: boolean
  expanded?: boolean
}

const { shown, warning, expanded } = defineProps<Props>()
const emit = defineEmits(['expand', 'collapse'])
</script>

<template>
  <DynamicIsland class="island" v-bind="{
    shown,
    expanded: shown && expanded,
    warning,
    superLeading: true,
    superTrailing: true
  }">
    <template #leading>
      <IconApple @click="emit('expand')" style="padding: 10px" />
    </template>
    <template #trailing>
      <IconTimer @click="emit('expand')" style="padding: 10px" />
    </template>
    <template #expanded>
      <div @click="emit('collapse')">
        Click to collapse
      </div>
    </template>
  </DynamicIsland>
</template>
