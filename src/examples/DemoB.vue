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
    superLeading: true
  }">
    <template #leading>
      <IconApple @click="emit('expand')" style="padding: 10px" />
    </template>
    <template #trailing>
      <div @click="emit('expand')" style="padding: 10px">Ring</div>
    </template>
    <template #expanded>
      <div @click="emit('collapse')">
        Hello World!
      </div>
    </template>
    <template #expanded-trailing>
      <IconTimer :size="60" style="padding: 10px" />
    </template>
  </DynamicIsland>
</template>
