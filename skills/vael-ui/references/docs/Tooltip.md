# Tooltip

A small floating label that appears on hover or focus to explain an element.

```ts
import { Tooltip } from 'vael-ui' // or 'vael-ui/vapor'
```

`v-tooltip` (with `<TooltipHost />` mounted once) is the lighter form for most targets. Its value is a string, or `{ content, props, follow, side, align, sideOffset, openDelay, closeDelay, keys, beforeClose, forceMount }`. `content` is text or a component for markup, such as a status dot or a progress bar, with its `props`: `v-tooltip="{ content: ProjectTip, props: { project } }"`. It's drawn in the one shared tooltip, so it still opens at once and glides between targets. `follow: true` tracks a mouse or pen along a wide target, such as a timeline bar, and stops at its ends; from the keyboard it stays centred. The tooltip is for text: put links and buttons in a Popover.

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`triggerEl` | `TriggerRef` |  | External trigger ref: a raw element or a component that exposes `el`.
`side` | `'top' \| 'right' \| 'bottom' \| 'left' \| undefined` | `'top'` | Which side of the trigger the tooltip opens on.
`align` | `'start' \| 'end' \| 'center' \| undefined` | `'center'` | How the tooltip aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the trigger and the tooltip, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the tooltip along the alignment axis, in pixels.
`follow` | `boolean \| undefined` | `false` | While open, the tooltip tracks a mouse or pen along the trigger's edge, clamped to it, in place of `align` and `alignOffset`: for a wide trigger, such as a timeline bar.
`openDelay` | `number \| undefined` | `400` | Delay before a cold open, in milliseconds. Warm-group opens (another tooltip visible or recently hidden) are instant.
`closeDelay` | `number \| undefined` | `100` | Grace period after the pointer leaves, in milliseconds, so it can move onto the tooltip.
`interactive` | `boolean \| undefined` | `true` | Hovering the tooltip itself keeps it open, so its text can be selected. Keyboard users can't reach it, so put links and buttons in a Popover.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the tooltip.
`beforeClose` | `BeforeClose<TooltipCloseReason> \| undefined` |  | Runs before the tooltip closes. Return `true` to close it, `false` to keep it open, or a promise of either, like `() => confirmAction(…).result`. The tooltip stays open while the promise is pending.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted while closed, toggled with `v-show`, so its content keeps its state. vael-ui then leaves its motion to you.
`motion` | `TooltipMotion \| undefined` |  | How it moves. `false` shows and hides it at once. An object moves it your way, like Popover's: values for the `panel` part, `{ spring }`, or `enter` / `leave` hooks. A warm open, right after another tooltip, stays instant either way.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element.
`ui` | `Partial<{ positioner: UiPartValue; panel: UiPartValue; }> \| undefined` |  | Class and style overrides for each part. Set `--ui-tooltip-bg` and `--ui-tooltip-color` on `panel` to recolour it.
`open` | `boolean \| undefined` | `false` | Whether the tooltip is open.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ open: boolean; isClosing: boolean; }` | Tooltip content; receives `open` and `isClosing`.
`trigger` | `{ open: boolean; setTriggerEl: (el: any) => void; }` | Trigger markup; bind `:ref="setTriggerEl"` to the triggering element.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: TooltipOpenChangeDetails]` | Fires when a close is requested; call `details.cancel()` to keep the tooltip open.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | Panel element (`null` while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (`null` while closed).
`placement` | `Placement` | Resolved placement after flipping, e.g. `'top'` or `'bottom-start'`.
`positionerStyle` | `Record<string, string>` | Computed position styles applied to the positioner.
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`show` | `() => void` | Opens the tooltip immediately, skipping `openDelay`; revives it if it's mid-close.
`hide` | `() => void` | Closes the tooltip, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the tooltip open.

## Motion

`motion` takes `TooltipMotion` (exported from `'vael-ui'`); `false` turns it off. Tooltip's box: values or hooks for its parts, like Popover's. Warm opens stay instant.

- Parts: `positioner`, `panel`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `targets`, `morph`, `exitOrder`, `resize`, `onAfterEnter`, `onAfterLeave`

See the [Motion guide](../guides/motion.md) for what each key does.

