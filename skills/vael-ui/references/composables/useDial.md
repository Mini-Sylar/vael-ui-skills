# useDial

The headless engine behind `<Dial>`.

The headless engine behind `<Dial>`: a value you turn like a jog wheel, with no fixed sweep. Each `degreesPerStep` of rotation (15° by default, a mouse wheel's detent) is one `step`. Leave `min` and `max` unset for a value that keeps counting in either direction, or set either one to clamp that end. The dial's own rotation keeps following the pointer even when the value stops at a bound, so it never stalls under your finger. It renders nothing: bind `rootStyle` and draw from `--ui-dial-rotation` and, when both bounds are set, `--ui-dial-fraction`.

```ts
import { useDial, DIAL_DEFAULT_DEGREES_PER_STEP } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { ref, useTemplateRef } from 'vue'
import { useDial } from 'vael-ui'

const zoom = ref(100)
const dialEl = useTemplateRef<HTMLElement>('dial')
const { rootStyle, onDialPointerdown, onDialKeydown } = useDial(zoom, {
  dialEl,
  min: 10,
  step: 5,
})
</script>

<template>
  <div
    ref="dial"
    class="my-dial"
    role="slider"
    tabindex="0"
    aria-label="Zoom"
    :aria-valuenow="zoom"
    :style="rootStyle"
    @pointerdown="onDialPointerdown"
    @keydown="onDialKeydown"
  >
    <span class="my-dial-notch" />
  </div>
</template>

<style>
.my-dial-notch {
  rotate: var(--ui-dial-rotation);
}
</style>
```

## Parameters

Name | Type | Description
--- | --- | ---
`model` | `Ref<number>` | The value. Changes by `step` for every `degreesPerStep` of rotation, and on key presses.
`dialEl` | `Ref<HTMLElement \| null>` | The round element. Pointer angles are measured around its centre.
`min / max` | `MaybeRefOrGetter<number \| undefined>` | Unset by default, so the value is unbounded. Each side clamps on its own.
`step` | `MaybeRefOrGetter<number>` | Default `1`.
`degreesPerStep` | `MaybeRefOrGetter<number \| undefined>` | Default `DIAL_DEFAULT_DEGREES_PER_STEP` (15). Lower turns faster.
`disabled` | `MaybeRefOrGetter<boolean>` | Ignores pointer and keyboard input.
`onCommit` | `() => void` | Fires once per settled interaction: on release, or after each key press. `<Dial>` re-emits it as `change`.

## Returns

Name | Type | Description
--- | --- | ---
`isDragging` | `Ref<boolean>` | True from pointer down to release.
`rootStyle` | `ComputedRef<Record<string, string>>` | Bind on your root: `--ui-dial-rotation`, the dial's own rotation in degrees, which keeps counting past 360°, and `--ui-dial-fraction` (0 to 1) when both bounds are set.
`fraction` | `ComputedRef<number \| undefined>` | Progress from 0 to 1, or `undefined` when the range is open on either side.
`onDialPointerdown` | `(event: PointerEvent) => void` | Bind on the dial. It tracks the drag on the window until release.
`onDialKeydown` | `(event: KeyboardEvent) => void` | Bind on the focusable element.
