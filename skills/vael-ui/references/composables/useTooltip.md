# useTooltip

The headless engine behind `<Tooltip>`.

The headless tooltip behind `<Tooltip>`: open and close delays, a warm group in which the next tooltip opens at once and glides from the last one, hover onto the tooltip, Escape, and touch long-press. It positions through `useFloatingPosition` and listens on the trigger for you. It doesn't render or set `aria-describedby`. `<Tooltip>` adds the panel, the teleport, the ARIA wiring and the glide styles. Use it to attach the same behaviour to markup of your own.

```ts
import { useTooltip } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { shallowRef, useId, useTemplateRef } from 'vue'
import { useTooltip } from 'vael-ui'

const open = shallowRef(false)
const tipId = useId()
const triggerEl = useTemplateRef<HTMLElement>('trigger')
const positionerEl = useTemplateRef<HTMLElement>('positioner')
const { positionerStyle, transformOrigin } = useTooltip(open, {
  triggerEl,
  positionerEl,
  side: 'bottom',
})
</script>

<template>
  <button ref="trigger" :aria-describedby="open ? tipId : undefined">Copy</button>
  <Teleport to="body">
    <div v-if="open" :id="tipId" ref="positioner" role="tooltip" :style="positionerStyle">
      <div class="tip" :style="{ transformOrigin }">Copy to clipboard</div>
    </div>
  </Teleport>
</template>
```

## Parameters

Name | Type | Description
--- | --- | ---
`open` | `Ref<boolean>` | First argument. The tooltip writes to it as it opens and closes.
`triggerEl` | `Ref<HTMLElement \| null>` | The element the tooltip describes and anchors to. Removing it from the page closes the tooltip.
`positionerEl` | `Ref<HTMLElement \| null>` | The positioned wrapper. Hovering it keeps the tooltip open while `interactive` is on. Teleport it to the body: the style it gets is absolute, in page coordinates.
`side` | `MaybeRefOrGetter<'top' \| 'right' \| 'bottom' \| 'left'>` | Default `top`. It flips when there is no room.
`align` | `MaybeRefOrGetter<'start' \| 'center' \| 'end'>` | Default `center`.
`sideOffset` | `MaybeRefOrGetter<number>` | Gap from the trigger, in px. Default `8`.
`alignOffset` | `MaybeRefOrGetter<number>` | Shift along the aligned edge, in px. Default `0`.
`openDelay` | `MaybeRefOrGetter<number>` | Delay before a cold open, in ms. Default `400`. A warm open is always instant: another tooltip is visible, or one closed less than 300 ms ago.
`closeDelay` | `MaybeRefOrGetter<number>` | Grace period after the pointer leaves, in ms, long enough to move onto the tooltip. Default `100`.
`interactive` | `MaybeRefOrGetter<boolean>` | Default `true`: hovering the tooltip itself keeps it open, so its content can be selected or clicked.
`closeOnEsc` | `MaybeRefOrGetter<boolean>` | Default `true`. The tooltip takes the Escape before any dialog around it, so the dialog stays open.
`onOpenChange` | `(value: boolean, details: TooltipOpenChangeDetails) => void` | Fires before every close, with `value: false`. `details.reason` is `'pointer'`, `'focus'`, `'trigger'`, `'escape'` or `'programmatic'`. Call `details.cancel()` to keep the tooltip open. It doesn't fire on open.
`beforeClose` | `() => ((done: () => void) => void) \| undefined` | A getter for a close hook. When it returns a function, the close waits until you call `done`, for an exit animation of your own. `isClosing` is `true` meanwhile.

## Returns

Name | Type | Description
--- | --- | ---
`positionerStyle` | `Ref<Record<string, string>>` | Bind it on the positioner. It stays hidden until the first measurement, then sets `position`, `top`, `left`, `visibility` and a `translate` while gliding.
`panelStyle` | `Ref<Record<string, string>>` | Custom properties for the glide between triggers: `--ui-tooltip-morph-x`, `--ui-tooltip-morph-y` (the box scale) and `--ui-tooltip-content-opacity`. Bind it on the panel if you style the morph.
`placement` | `Ref<Placement>` | The resolved placement after flipping, such as `'top'` or `'bottom-start'`.
`transformOrigin` | `Ref<string>` | The trigger-facing origin for a scale-in animation.
`isClosing` | `Ref<boolean>` | `true` while a `beforeClose` hook holds the close.
`instant` | `Ref<boolean>` | `true` when this open was warm. Skip the enter animation then.
`traveling / morphing / travelActive` | `Ref<boolean>` | Glide state. `traveling` and `morphing` are on while the position and size transitions should run; `travelActive` covers the whole glide, from the staged first frame until it settles.
`show / hide` | `() => void` | `show` opens at once, skipping `openDelay`, and revives a pending close. `hide` closes through `onOpenChange` and `beforeClose` with the `programmatic` reason.
`requestClose` | `(reason: TooltipCloseReason, event?: Event) => void` | Closes with your own reason, through `onOpenChange` and `beforeClose`.
`cancelClose` | `() => void` | Drops a close that a `beforeClose` hook holds, and keeps the tooltip open.
