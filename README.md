# Vue 3 + TypeScript + Vite

This template should help get you started developing with Vue 3 and TypeScript in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VS Code](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar)

## Type Support For `.vue` Imports in TS

Since TypeScript cannot handle type information for `.vue` imports, they are shimmed to be a generic Vue component type by default. In most cases this is fine if you don't really care about component prop types outside of templates. However, if you wish to get actual prop types in `.vue` imports (for example to get props validation when using manual `h(...)` calls), you can enable Volar's Take Over mode by following these steps:

1. Run `Extensions: Show Built-in Extensions` from VS Code's command palette, look for `TypeScript and JavaScript Language Features`, then right click and select `Disable (Workspace)`. By default, Take Over mode will enable itself if the default TypeScript extension is disabled.
2. Reload the VS Code window by running `Developer: Reload Window` from the command palette.

You can learn more about Take Over mode [here](https://github.com/johnsoncodehk/volar/discussions/471).

<!--
  TODO:
  API design
  - props
    left: 'normal' | 'outer' | 'max'
    right: 'normal' | 'outer' | 'max'
    shown: true | false
    expanded: true | false
    layout: 'normal' | 'large' | 'square'
    leftResponsive: true | false
    rightResponsive: true | false
    warning: true | false
  - slots
    left
    right
    expanded
    expanded-left
    expanded-right
  - events
  Internal design
  - structure
    content: shown -> warning
      left: shown -> normal|outer|max, leftResponsive
        bg-left: cover left
      right: shown -> normal|outer|max, rightResponsive
        bg-right: cover right
      main: shown -> expanded -> normal|large|square
        main-left
        main-right
        bg-main: cover main
  - transition
    warning
      shake
    show
      fg(translate(bouncing) + blur)
      bg(size/translate(bouncing) + blobbing)
    expand
      fg(opacity + blur + scale(bouncing))
      bg(radius + size(bouncing))
      left/right(scale/translate(bouncing))
    note
      don't bounce (top line, scale down) to avoid explosing forbidden zone
-->
