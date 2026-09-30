# Tooltip

A small floating label that appears on hover or focus to explain an element.

```ts
import { Tooltip } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`triggerEl` | `TriggerRef` |  | External trigger ref: a raw element or a component that exposes `el`.
`side` | `Side \| undefined` | `'top'` | Which side of the trigger the tooltip opens on.
`align` | `Align \| undefined` | `'center'` | How the tooltip aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the trigger and the tooltip, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the tooltip along the alignment axis, in pixels.
`openDelay` | `number \| undefined` | `400` | Delay before a cold open, in milliseconds. Warm-group opens (another tooltip visible or recently hidden) are instant.
`closeDelay` | `number \| undefined` | `100` | Grace period after the pointer leaves, in milliseconds, so it can move onto the tooltip.
`interactive` | `boolean \| undefined` | `true` | Hovering the tooltip itself keeps it open, so its content stays selectable and clickable.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the tooltip.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted, toggled with `v-show`, so you can own the enter/exit animation.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element.
`ui` | `Partial<{ positioner: UiPartValue; panel: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
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

