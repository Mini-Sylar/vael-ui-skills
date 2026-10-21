# useKnob

The headless engine behind `<Knob>`.

The headless engine behind `<Knob>`: a value on a 270° sweep, from −135° to +135° with the gap at the bottom. Dragging reads the pointer's angle around the dial's centre, so the value follows your finger round the circle instead of a straight-line distance. Arrow keys step, Page Up and Page Down jump 10 steps, Home and End go to the bounds. It renders nothing: bind `rootStyle` on your root and draw the dial from `--ui-knob-angle` and `--ui-knob-fraction`. The `KNOB_SWEEP_*` constants give the arc in degrees, for drawing a track that matches.

```ts
import { useKnob, KNOB_SWEEP_START_DEG, KNOB_SWEEP_DEG, KNOB_SWEEP_END_DEG } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { ref, useTemplateRef } from 'vue'
import { useKnob } from 'vael-ui'

const volume = ref(40)
const dialEl = useTemplateRef<HTMLElement>('dial')
const { rootStyle, onDialPointerdown, onDialKeydown } = useKnob(volume, {
  dialEl,
  min: 0,
  max: 100,
  onCommit: () => console.log('saved', volume.value),
})
</script>

<template>
  <div
    ref="dial"
    class="my-knob"
    role="slider"
    tabindex="0"
    aria-label="Volume"
    aria-valuemin="0"
    aria-valuemax="100"
    :aria-valuenow="volume"
    :style="rootStyle"
    @pointerdown="onDialPointerdown"
    @keydown="onDialKeydown"
  >
    <span class="my-knob-pointer" />
  </div>
</template>

<style>
.my-knob-pointer {
  rotate: var(--ui-knob-angle);
}
</style>
```

## Parameters

Name | Type | Description
--- | --- | ---
`model` | `Ref<number>` | The value. Set on every drag frame and key press, clamped and snapped to `step`.
`dialEl` | `Ref<HTMLElement \| null>` | The round element. Pointer angles are measured around its centre.
`min / max` | `MaybeRefOrGetter<number>` | Default `0` and `100`. The sweep always spans this whole range.
`step` | `MaybeRefOrGetter<number>` | Default `1`. Values snap to it, without float drift such as 0.30000000000000004.
`disabled` | `MaybeRefOrGetter<boolean>` | Ignores pointer and keyboard input.
`onCommit` | `() => void` | Fires once per settled interaction: on release, or after each key press. `<Knob>` re-emits it as `change`.

## Returns

Name | Type | Description
--- | --- | ---
`isDragging` | `Ref<boolean>` | True from pointer down to release.
`rootStyle` | `ComputedRef<Record<string, string>>` | Bind on your root: `--ui-knob-angle` (−135deg to 135deg) and `--ui-knob-fraction` (0 to 1). Draw the pointer's rotation and the fill arc from them in CSS.
`onDialPointerdown` | `(event: PointerEvent) => void` | Bind on the dial. It tracks the drag on the window until release, so leaving the dial mid-drag keeps it going.
`onDialKeydown` | `(event: KeyboardEvent) => void` | Bind on the focusable element: arrows, Page Up, Page Down, Home and End.
