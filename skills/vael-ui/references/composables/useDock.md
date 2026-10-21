# useDock

The headless engine behind `<Dock>`.

The magnification engine behind `<Dock>`. Items near the pointer grow along a smooth cosine falloff, and their neighbours spread apart to make room. It writes `transform`, or the real size in `grow` mode, straight onto each item every animation frame, so the effect costs no re-renders. A mouse magnifies on hover. A finger or pen magnifies once it slides 8 px along the dock, so a tap stays a tap. It stays at rest under `prefers-reduced-motion: reduce`. `<Dock>` adds the buttons, tooltips, roving keyboard focus and the tap bounce.

```ts
import { useDock } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { useTemplateRef } from 'vue'
import { useDock } from 'vael-ui'

const apps = ['Mail', 'Notes', 'Music', 'Photos']
const rootEl = useTemplateRef<HTMLElement>('root')
const { setItemEl, onPointerDown, onPointerMove, onPointerLeave } = useDock(
  rootEl,
  () => apps.length,
  { baseSize: 48, maxSize: 72 },
)
</script>

<template>
  <div
    ref="root"
    class="my-dock"
    @pointerdown="onPointerDown"
    @pointermove="onPointerMove"
    @pointerleave="onPointerLeave"
    @pointerup="onPointerLeave"
    @pointercancel="onPointerLeave"
  >
    <button v-for="(app, i) in apps" :key="app" :ref="setItemEl(i)" class="my-dock-item">
      {{ app }}
    </button>
  </div>
</template>

<style>
/* touch-action: none lets a finger slide along the dock without scrolling the page. */
.my-dock { display: flex; gap: 8px; touch-action: none; }
.my-dock-item { inline-size: 48px; block-size: 48px; }
</style>
```

## Parameters

Name | Type | Description
--- | --- | ---
`rootEl` | `Ref<HTMLElement \| null>` | First argument. The dock. Item centres and its `gap` are measured against it.
`itemCount` | `MaybeRefOrGetter<number>` | Second argument. A change re-measures the items after the next render.
`orientation` | `MaybeRefOrGetter<'horizontal' \| 'vertical'>` | Default `'horizontal'`. Sets the axis items grow and spread along.
`baseSize` | `MaybeRefOrGetter<number>` | Default `48`. The resting item size in px that `maxSize` and `range` are designed against. Items that rest smaller, in a narrow container, magnify in proportion.
`maxSize` | `MaybeRefOrGetter<number>` | Default `76`. The item size in px directly under the pointer.
`range` | `MaybeRefOrGetter<number \| undefined>` | Default `baseSize * 3.5`. The distance in px from the pointer at which items are back to `baseSize`.
`magnify` | `MaybeRefOrGetter<boolean>` | Default `true`. `false` keeps every item at rest.
`disabled` | `MaybeRefOrGetter<boolean>` | Default `false`. Here it only turns magnification off, the same as `magnify: false`. Blocking clicks is up to your markup.
`grow` | `MaybeRefOrGetter<boolean>` | Default `false`, which scales and shifts items with `transform`: the dock keeps its size and end items can reach past it. `true` sets each item's real `inline-size` and `block-size`, so the dock grows with them.

## Returns

Name | Type | Description
--- | --- | ---
`setItemEl` | `(index: number) => (el: unknown) => void` | Bind it as `:ref="setItemEl(index)"` on each item. A component instance resolves to its root element.
`onPointerMove` | `(event: PointerEvent) => void` | Bind it on the root `pointermove`. It retargets every item.
`onPointerLeave` | `() => void` | Bind it on the root `pointerleave`, `pointerup` and `pointercancel`. Items ease back to rest.
`onPointerDown` | `(event: PointerEvent) => void` | Bind it on the root `pointerdown`. It records where a finger or pen landed, for the 8 px slide check.
`touchSlid` | `() => boolean` | `true` once the finger or pen that is down has slid far enough to magnify. `false` means the press is a tap, so you can tell a slide from a tap on release.
`remeasure` | `() => void` | Re-measures the resting item centres. It already runs on mount, on root resize and when `itemCount` changes. Call it after a layout change none of those catch, such as a font swap.
