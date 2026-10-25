# usePopover

The headless engine behind `<Popover>`.

The headless popover behind `<Popover>`, `<Menu>`, `<Select>`, `<Combobox>`, `<DatePicker>` and `<TreeSelect>`. It positions the panel, closes on Escape and on outside pointer down or focus, joins the shared layer stack so only the topmost layer closes, and returns focus when the panel closes. It doesn't open anything or toggle on a trigger click: you own `open`. `<Popover>` adds the trigger slot, teleport, panel, z-index and animation.

```ts
import { usePopover } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { shallowRef, useTemplateRef } from 'vue'
import { usePopover } from 'vael-ui'

const open = shallowRef(false)
const triggerEl = useTemplateRef<HTMLElement>('trigger')
const positionerEl = useTemplateRef<HTMLElement>('positioner')
const { positionerStyle, close } = usePopover(open, {
  triggerEl,
  positionerEl,
  side: 'bottom',
  align: 'start',
  tabIntoPanel: true,
})
</script>

<template>
  <button ref="trigger" :aria-expanded="open" @click="open = !open">Filters</button>
  <Teleport to="body">
    <div v-if="open" ref="positioner" :style="positionerStyle">
      <label><input type="checkbox" /> In stock</label>
      <button @click="close">Done</button>
    </div>
  </Teleport>
</template>
```

## Parameters

Name | Type | Description
--- | --- | ---
`open` | `Ref<boolean>` | First argument. You set it to `true`; the popover sets it to `false` when it closes.
`triggerEl` | `Ref<HTMLElement \| null>` | The anchor. Pointer down on it never counts as outside, so your own click toggle works. If focus was in the panel when it closes, focus goes back to the element that had it before opening, or to the trigger.
`positionerEl` | `Ref<HTMLElement \| null>` | The positioned panel wrapper. Teleport it to the body.
`side` | `MaybeRefOrGetter<'top' \| 'right' \| 'bottom' \| 'left'>` | Default `bottom`. It flips when there is no room.
`align` | `MaybeRefOrGetter<'start' \| 'center' \| 'end'>` | Default `center`.
`sideOffset / alignOffset` | `MaybeRefOrGetter<number>` | Gap from the trigger and shift along it, in px. Defaults `8` and `0`.
`matchReferenceWidth` | `MaybeRefOrGetter<boolean>` | Makes the panel at least as wide as the trigger, as `<Select>` does. Default `false`.
`maxHeightCap` | `MaybeRefOrGetter<number \| undefined>` | Caps the returned `maxHeight` in px, even when the viewport has more room.
`closeOnEsc` | `MaybeRefOrGetter<boolean>` | Default `true`. Only the topmost layer closes.
`closeOnOutside` | `MaybeRefOrGetter<boolean>` | Default `true`. Closes on pointer down or focus outside the trigger and panel, and when Tab leaves the panel.
`onOpenChange` | `(value: boolean, details: PopoverOpenChangeDetails) => void` | Fires before every close, with `value: false`. `details.reason` is `'trigger'`, `'escape'`, `'outside'` or `'programmatic'`. Call `details.cancel()` to keep it open. It doesn't fire on open.
`beforeClose` | `() => BeforeClose \| undefined` | A getter for the close guard. Return `true` to close, `false` or nothing to stay open, or a promise of either. `isClosing` is `true` while a promise is pending.
`scope` | `Ref<HTMLElement \| null>` | The region this popover belongs to, for Escape ownership in the layer stack. Omit it for a page-level popover.
`tabIntoPanel` | `boolean` | Tab from the trigger moves into the panel, and Tab past either end closes it and carries on from the trigger. Turn it on for panels that take focus. Leave it off when focus stays on the trigger, as in a combobox. Not reactive.

## Returns

Name | Type | Description
--- | --- | ---
`positionerStyle` | `Ref<Record<string, string>>` | Bind it on the positioner. Hidden until the first measurement.
`placement` | `Ref<Placement>` | The resolved placement after flipping, such as `'bottom-start'`.
`transformOrigin` | `Ref<string>` | The trigger-facing origin for a scale-in animation.
`maxHeight` | `Ref<number \| null>` | The room left on the chosen side, in px. Put it on a scrolling body inside the panel.
`isClosing` | `Ref<boolean>` | `true` while a `beforeClose` hook holds the close.
`close` | `() => void` | Closes with the `'programmatic'` reason, through `onOpenChange` and `beforeClose`.
`requestClose` | `(reason: PopoverCloseReason, event?: Event) => void` | Closes with your own reason. `<Popover>` uses `'trigger'` for its trigger click.
`cancelClose` | `() => void` | Drops a close that a `beforeClose` hook holds.
`layerIndex` | `() => number` | This popover's position in the shared layer stack, or `-1` while closed. `<Popover>` adds it to `--ui-z-dialog`, so later layers stack on top.
