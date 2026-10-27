# Popover

A floating panel anchored to a trigger element, for menus, forms, or extra detail.

```ts
import { Popover } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`ui` | `Partial<{ positioner: UiPartValue; panel: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`triggerEl` | `TriggerRef` |  | External trigger ref: a raw element or a component that exposes `el`. Use `#trigger` if the trigger can live here.
`side` | `'top' \| 'right' \| 'bottom' \| 'left' \| undefined` | `'bottom'` | Which side of the trigger the panel opens on.
`align` | `'start' \| 'end' \| 'center' \| undefined` | `'center'` | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | `true` | Clicking outside the panel closes it.
`beforeClose` | `BeforeClose<PopoverCloseReason> \| undefined` |  | Runs before the panel closes. Return `true` to close it, `false` to keep it open, or a promise of either, like `() => confirmAction(…).result`. The panel stays open while the promise is pending.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted while closed, toggled with `v-show`, so its content keeps its state. vael-ui then leaves its motion to you.
`teleportTo` | `string \| HTMLElement \| undefined` |  | Teleport target: a CSS selector or element. Wins over `container` either way.
`scrollFade` | `boolean \| undefined` | `true` | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`container` | `DOMTarget \| undefined` |  | Scopes the popover to an element. It teleports there instead of `body`, and its Escape handling stays within it. Omit it for a page-level popover.
`openOnTriggerClick` | `boolean \| undefined` | `false` | Clicking the `#trigger` slot toggles `open`, and the popover finds the trigger element itself (no `setTriggerEl` needed). Otherwise you drive `open` yourself.
`motion` | `PopoverMotion<N> \| undefined` |  | How it moves: `false` for instantly, `{ spring }` to retime the built-in motion, states by part (`initial: { panel: { … } }`), your `targets`, or `enter`/`leave` hooks for a library. Unset, it follows the nearest ConfigProvider's style.
`open` | `boolean \| undefined` | `false` | Whether the popover is open.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ close: () => void; open: boolean; isClosing: boolean; cancelClose: () => void; panelEl: HTMLElement \| null; placement: string; target: (name: N) => MotionRef; phase: MotionPhase; }` | Panel content; receives `close`, `open`, `isClosing`, `cancelClose`, `panelEl` and `placement`.
`trigger` | `{ open: boolean; setTriggerEl: (el: any) => void; }` | Trigger markup; bind `:ref="setTriggerEl"` to position against it. Clicking it does nothing unless you set `openOnTriggerClick`.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires when a close is requested; call `details.cancel()` to keep the panel open.
`after-enter` | `[]` | Fires once it has finished opening.
`after-leave` | `[]` | Fires once it has finished closing.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | Panel element (`null` while closed).
`phase` | `MotionPhase` | `'entering' \| 'entered' \| 'leaving' \| 'left'`.
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (`null` while closed).
`placement` | `Placement` | Resolved placement after flipping, e.g. `'bottom-start'`.
`positionerStyle` | `Record<string, string>` | Computed position styles applied to the positioner.
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`close` | `() => void` | Closes the panel, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the panel open.

## Motion

`motion` takes `PopoverMotion` (exported from `'vael-ui'`); `false` turns it off. Popover's `motion`: values or hooks for `positioner` and `panel`, how it opens and closes (`morph`, `surface`, `resize`) and swapped content.

- Parts: `positioner`, `panel`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `targets`, `morph`, `exitOrder`, `resize`, `surface`, `onAfterEnter`, `onAfterLeave`, `swap`, `direction`, `onAfterSwap`

See the [Motion guide](../guides/motion.md) for what each key does.

