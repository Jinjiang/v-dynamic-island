# Vue Dynamic Island

A Vue (3.x) implementation of Dynamic Island.

## How to use

Install first:

```bash
pnpm install v-dynamic-island
```

Then import into your Vue project:

```vue
<script setup>
import { ref } from 'vue'
import DynamicIsland from 'v-dynamic-island'
import 'v-dynamic-island/style.css'

// to control the display
const shown = ref(false)
const expanded = ref(false)
</script>

<template>
  <DynamicIsland :shown="shown" :expanded="expanded">
    <template #leading>
      <!-- leading content -->
    </template>
    <template #trailing>
      <!-- trailing content -->
    </template>
    <template #expanded>
      <!-- expanded content -->
    </template>
  </DynamicIsland>
</template>
```

## Explanations

The `shown` prop controls whether the island would be enabled.

The island has two modes: compat mode and expanded mode. The `expanded` prop controls which mode it is on.

The `#leading` and `#trailing` slots accepts the compat content. The former one is for the left side and the later one is for the right side.

The `#expanded` slot accepts the expanded content.

To be notice that the "little-pill" core area (roughly `137px` x `40px`) of the island always shouldn't be able to render anything since that's the place where Apple puts the TrueDepth Camera.

## Live examples

https://jinjiang.dev/v-dynamic-island

## Basic APIs

### Props

- `shown`: `boolean`
- `expanded`: `boolean`

### Slots

- `#leading`
- `#trailing`
- `#expanded`

### Emits

- none

## Advanced feature

- Shaking as a warning.
- Customize device width for the max width of expanded content.
- "Super element" transitions for leading/trailing content between compat mode and expanded mode.
- Slots for expanded leading/trailing content.

## Advanced APIs

### Props

- `warning`: `boolean`

  When set to be `true`, the whole island would be shaked for a few times as a warning. According to the original design of Dynamic Island, this prop should only be used on the compat mode.

- `deviceWidth`: `number`

  By default the component will auto detect the device width. However, you can explicitly specify a number for that. Usually, it's useful when you prepare a demo on non-iOS devices or laptops.

- `superLeading`: `boolean` or `{ top, left, size }`

  By default, `#leading` content will be hidden on the expanded mode. However, if you want to retain it, you can set it to be `true`. So when switching to the expanded mode, `#leading` content would be kept and scaled up.

  You can also set it as a customized position and size for its expanded mode.
  
  - The `top` and `left` is the number of pixels from the center of the content to the island edges.
  - The `size` is the number of pixels height, which is used for calculating the scaling transition.

- `superTrailing`: `boolean` or `{ top, right, size }`

  Similar to `superTrailing` but for `#trailing` content.

### Slots

- `#expanded-leading`
- `#expanded-trailing`

### Emits

- none

## Specs & default values

TrueDepth Camera:
- Size of the cemara: `137px` x `40px`.

Compat mode:
- Gap between heading/trailing slots and the TrueDepth Camera: `10px`.
- Height of headling/trailing slots: `40px`.

Expanded mode:
- Padding of the expanded slot: top `137px`, other sides `10px`.
- Default height of headling/trailing slots: `80px`.
- Default center of heading slot to the top: `46px`.
- Default center of heading slot to the left: `46px`.
- Default center of trailing slot to the top: `46px`.
- Default center of trailing slot to the right: `46px`.
