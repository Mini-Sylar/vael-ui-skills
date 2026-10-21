# useResizable

The headless engine behind `<Resizable>`.

The headless drag and keyboard engine behind `<Resizable>`. It turns a handle's pointer drag into a size in px on your ref. Mid drag, the size stretches past `min` and `max` with rubber-band resistance, then settles back within them on release. Arrow keys resize by 16 px, and Home and End jump to `min` and `max`. `<Resizable>` adds the panel, the separator role and its ARIA values. Use the composable when the handle and the panel live apart in your own layout.

```ts
import { useResizable } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { ref } from 'vue'
import { useResizable } from 'vael-ui'

const width = ref(240)
const { isDragging, onHandlePointerdown, onHandleKeydown } = useResizable(width, {
  min: 160,
  max: 480,
})
</script>

<template>
  <aside class="sidebar" :style="{ inlineSize: `${width}px` }">
    <nav>...</nav>
    <div
      class="sidebar-handle"
      role="separator"
      aria-orientation="vertical"
      aria-label="Resize sidebar"
      tabindex="0"
      :aria-valuenow="Math.round(width)"
      aria-valuemin="160"
      aria-valuemax="480"
      :data-active="isDragging || undefined"
      @pointerdown="onHandlePointerdown"
      @keydown="onHandleKeydown"
    />
  </aside>
</template>
```

## Parameters

Name | Type | Description
--- | --- | ---
`model` | `Ref<number>` | First argument. The size in px, written on every drag frame.
`min` | `MaybeRefOrGetter<number>` | Default `0`.
`max` | `MaybeRefOrGetter<number>` | Default `Infinity`, no limit. End jumps the size to `max` when there is one, and does nothing without it.
`direction` | `MaybeRefOrGetter<'horizontal' \| 'vertical'>` | Default `'horizontal'`, which reads `clientX` and the Left and Right arrows. `'vertical'` reads `clientY` and the Up and Down arrows.
`edge` | `MaybeRefOrGetter<'start' \| 'end'>` | Default `'end'`: the handle sits on the trailing edge, so dragging right or down grows the panel. `'start'` flips it, for a handle on a right-docked panel's left border.
`disabled` | `MaybeRefOrGetter<boolean>` | Default `false`. Ignores pointer and key input while `true`.
`onCommit` | `() => void` | Fires once per settled interaction: on pointer release, or after each key press. Persist the size here rather than on every drag frame.

## Returns

Name | Type | Description
--- | --- | ---
`onHandlePointerdown` | `(event: PointerEvent) => void` | Bind it on the handle. Give the handle `touch-action: none`, so a touch drag resizes instead of scrolling.
`onHandleKeydown` | `(event: KeyboardEvent) => void` | Bind it on the handle, and give the handle `tabindex="0"` so it can take focus.
`isDragging` | `Ref<boolean>` | `true` from pointer down until release.
