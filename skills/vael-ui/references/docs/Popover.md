# Popover

A floating panel anchored to a trigger element, for menus, forms, or extra detail.

```ts
import { Popover } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`triggerEl` | `TriggerRef` |  | External trigger ref: a raw element or a component that exposes `el`. Use `#trigger` if the trigger can live here.
`side` | `Side \| undefined` | `'bottom'` | Which side of the trigger the panel opens on.
`align` | `Align \| undefined` | `'center'` | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | `true` | Clicking outside the panel closes it.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted, toggled with `v-show`, so you can own the enter/exit animation.
`teleportTo` | `string \| HTMLElement \| undefined` |  | Teleport target: a CSS selector or element. Wins over `container` either way.
`container` | `DOMTarget \| undefined` |  | Scopes the popover to an element. It teleports there instead of `body`, and its Escape handling stays within it. Omit it for a page-level popover.
`scrollFade` | `boolean \| undefined` | `true` | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`openOnTriggerClick` | `boolean \| undefined` | `false` | Clicking the `#trigger` slot toggles `open`, and the popover finds the trigger element itself (no `setTriggerEl` needed). Otherwise you drive `open` yourself.
`ui` | `Partial<{ positioner: UiPartValue; panel: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the popover is open.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ close: () => void; open: boolean; isClosing: boolean; cancelClose: () => void; panelEl: HTMLElement \| null; placement: string; }` | Panel content; receives `close`, `open`, `isClosing`, `cancelClose`, `panelEl` and `placement`.
`trigger` | `{ open: boolean; setTriggerEl: (el: any) => void; }` | Trigger markup; bind `:ref="setTriggerEl"` to position against it. Clicking it does nothing unless you set `openOnTriggerClick`.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires when a close is requested; call `details.cancel()` to keep the panel open.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | Panel element (`null` while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (`null` while closed).
`placement` | `Placement` | Resolved placement after flipping, e.g. `'bottom-start'`.
`positionerStyle` | `Record<string, string>` | Computed position styles applied to the positioner.
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`close` | `() => void` | Closes the panel, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the panel open. 

