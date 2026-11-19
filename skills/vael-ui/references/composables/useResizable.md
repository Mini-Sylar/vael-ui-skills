# useResizable

The headless engine behind `<Resizable>`.

The headless drag and keyboard engine behind `<Resizable>`. It turns a handle's pointer drag into a size in px on your ref, written once a frame. Mid drag, the size stretches past `min` and `max` with rubber-band resistance, then settles back within them on release. Escape puts it back where the drag started. Arrow keys resize by 16 px, and Home and End jump to `min` and `max`. `<Resizable>` adds the panel, the separator role and its ARIA values. Use the composable when the handle and the panel live apart in your own layout.

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

A span, such as a timeline bar: one instance with `offset`, and each end's handle names its edge. Drag either end or the bar; it snaps to a day on release, Escape puts it back, and the timeline scrolls near its edges.

```ts
const DAY = 28
const scroller = useTemplateRef<HTMLElement>('scroller') // the scrolling timeline
const start = shallowRef(4 * DAY)
const size = shallowRef(10 * DAY)
const bar = useResizable(size, { offset: start, bounds: [0, 60 * DAY], step: DAY, min: DAY, scrollEl: scroller })
// bar: :style="{ insetInlineStart: `${start}px`, inlineSize: `${size}px` }"
//      @pointerdown="bar.onMovePointerdown" @keydown="bar.onMoveKeydown"
// start end: @pointerdown.stop="bar.onHandlePointerdown($event, 'start')" @keydown.stop="bar.onHandleKeydown($event, 'start')"
// end end:   the same with 'end'
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
`step` | `MaybeRefOrGetter<number \| undefined>` | Snaps to a multiple of `step` on release, a day on a timeline say, while the drag itself stays smooth. An arrow key moves one step. Unset, a drag doesn't snap and an arrow key moves 16 px.
`offset` | `Ref<number>` | Where it starts, for a span such as a timeline bar. A handle for the `'start'` edge then moves the start and the size together, so the far end stays put, and `onMovePointerdown` moves the whole span. Two instances over the same two refs, one per edge, make a bar. A handle pulled past `min` flips the span over its other end.
`centered` | `MaybeRefOrGetter<boolean>` | Resizes from the middle: the edge you pull follows the pointer and the opposite edge moves the same distance the other way, so the size changes twice as far and the centre stays put. With `offset`, the start moves to keep it there. Default `false`.
`flip` | `MaybeRefOrGetter<boolean>` | With `offset`: a handle pulled past `min` flips the span over its other end, so one press can pull a span out either way. Default `true`. Set `false` to stop at `min` instead, as a panel does.
`bounds` | `MaybeRefOrGetter<[number, number]>` | With `offset`: the span stays between these, start and end.
`scrollEl` | `MaybeRefOrGetter<HTMLElement \| null>` | A scrolling container. Near its edges it scrolls under the pointer during a drag, faster the closer the pointer gets, and the drag follows. Set its `scroll-padding` to what covers it, such as a sticky column, and the edge is measured from there.
`onCommit` | `() => void` | Fires once per settled interaction: on pointer release, or after each key press. Persist the size here rather than on every drag frame.

## Returns

Name | Type | Description
--- | --- | ---
`onMovePointerdown` | `(event: PointerEvent) => void` | With `offset`: bind it on the span to move it whole. It starts after 4 px of travel, so a still press stays a click, and the click after a move, or after Escape, doesn't fire.
`onMoveKeydown` | `(event: KeyboardEvent) => void` | With `offset`: the arrow keys move the whole span a step.
`onHandlePointerdown` | `(event: PointerEvent, edge?: 'start' \| 'end') => void` | Bind it on the handle. Give the handle `touch-action: none`, so a touch drag resizes instead of scrolling. A span has two handles: name the one you pressed. Left out, it's the `edge` option.
`onHandleKeydown` | `(event: KeyboardEvent, edge?: 'start' \| 'end') => void` | Bind it on the handle, and give the handle `tabindex="0"` so it can take focus. Name its edge as `onHandlePointerdown` does.
`isDragging` | `Ref<boolean>` | `true` from pointer down until release. A move counts from its first 4 px.
