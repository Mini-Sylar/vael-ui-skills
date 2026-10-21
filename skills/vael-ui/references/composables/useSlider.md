# useSlider

The headless engine behind `<Slider>`.

The headless engine behind `<Slider>`. It maps pointer and keyboard input onto a number or a `[start, end]` tuple, snaps to `step` and keeps two thumbs from crossing. It writes no inline style on the thumbs or the fill. Instead, `rootStyle` sets two 0 to 1 fractions, `--ui-slider-p0` and `--ui-slider-p1`, and your CSS places everything from them. `<Slider>` adds the markup, ARIA, labels and the focus ring. Use the composable when you need a track a component can't give you, such as a scrubber or a custom range picker.

```ts
import { useSlider } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { ref, useTemplateRef } from 'vue'
import { useSlider } from 'vael-ui'

const volume = ref(40)
const trackEl = useTemplateRef<HTMLElement>('track')
const { rootStyle, onTrackPointerdown, onThumbPointerdown, onThumbKeydown } = useSlider(volume, {
  trackEl,
  step: 5,
})
</script>

<template>
  <div class="volume" :style="rootStyle">
    <div ref="track" class="volume-track" @pointerdown="onTrackPointerdown">
      <span
        class="volume-thumb"
        role="slider"
        tabindex="0"
        aria-label="Volume"
        :aria-valuenow="volume"
        aria-valuemin="0"
        aria-valuemax="100"
        @pointerdown.stop="onThumbPointerdown(0, $event)"
        @keydown="onThumbKeydown(0, $event)"
      />
    </div>
  </div>
</template>

<style>
/* Without touch-action: none, a touch drag scrolls the page instead. */
.volume-track { position: relative; block-size: 4px; touch-action: none; }
.volume-thumb {
  position: absolute;
  inset-inline-start: calc(var(--ui-slider-p0) * 100%);
  translate: -50% 0;
}
</style>
```

## Parameters

Name | Type | Description
--- | --- | ---
`model` | `Ref<number \| [number, number]>` | First argument. The value the slider writes to. A tuple turns on range mode with two thumbs that can touch but never cross.
`trackEl` | `Ref<HTMLElement \| null>` | The track. Pointer positions map against its rect, so the value follows the pointer 1:1 along it.
`thumbEls` | `Ref<HTMLElement[] \| null>` | The thumbs in index order. When you pass them, a drag or a track press focuses the thumb it moves.
`min` | `MaybeRefOrGetter<number>` | Default `0`.
`max` | `MaybeRefOrGetter<number>` | Default `100`.
`step` | `MaybeRefOrGetter<number>` | Default `1`. Every value snaps to it, counted from `min`. Arrow keys move one step, Page Up and Page Down move ten.
`orientation` | `MaybeRefOrGetter<'horizontal' \| 'vertical'>` | Default `'horizontal'`. A vertical track counts up from its bottom edge.
`disabled` | `MaybeRefOrGetter<boolean>` | Default `false`. Ignores every pointer and key handler while `true`.
`onCommit` | `() => void` | Fires once per settled interaction: on pointer release, or after each key press. `model` updates on every drag frame, so defer an expensive side effect, such as a save request, to here. `<Slider>` re-emits it as `change`, like a native range input.

## Returns

Name | Type | Description
--- | --- | ---
`rootStyle` | `ComputedRef<Record<string, string>>` | Bind it on the root. It sets `--ui-slider-p0`, the first value as a 0 to 1 fraction of the range, and `--ui-slider-p1` in range mode.
`onTrackPointerdown` | `(event: PointerEvent) => void` | Bind it on the track. It jumps the nearest thumb to the press and keeps dragging it until release.
`onThumbPointerdown` | `(index: number, event: PointerEvent) => void` | Bind it on each thumb with `.stop`, so the track handler does not run as well.
`onThumbKeydown` | `(index: number, event: KeyboardEvent) => void` | Bind it on each thumb. Handles the arrow keys, Page Up, Page Down, Home and End.
`isDragging` | `Ref<boolean>` | `true` from pointer down until release.
`activeThumb` | `Ref<number \| null>` | The index of the thumb a pointer is dragging, or `null` when idle.
