<script setup lang="ts">
import DynamicIsland from '../components/DynamicIsland.vue';
import Avatar from './Avatar.vue'
import Label from './Label.vue'
import LargeContent from './LargeContent.vue'

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
    superLeading: {
      top: 60,
      left: 60,
      size: 100
    }
  }">
    <template #leading>
      <Avatar @click="emit('expand')" />
    </template>
    <template #trailing>
      <div style="padding: 10px">Ring</div>
    </template>
    <template #expanded>
      <LargeContent @click="emit('collapse')" />
    </template>
    <template #expanded-trailing>
      <Label />
    </template>
  </DynamicIsland>
</template>
