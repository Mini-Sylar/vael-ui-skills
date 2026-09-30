# ContextMenu

A menu that opens at the pointer on right-click, replacing the native browser menu.

```ts
import { ContextMenu } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly MenuEntry<T>[] \| undefined` |  | Data-driven rows, in the same shape as Menu's `items`.
`disabled` | `boolean \| undefined` | `false` | Disables both the right-click and long-press triggers.
`longPress` | `boolean \| undefined` | `true` | Adds a touch long-press trigger alongside the native `contextmenu` event.
`longPressDelay` | `number \| undefined` | `500` | How long you hold a touch press, in ms, before the menu opens.
`side` | `Side \| undefined` | `'bottom'` | Which side of the cursor point the panel opens on.
`align` | `Align \| undefined` | `'start'` | How the panel aligns against the cursor point along that side.
`sideOffset` | `number \| undefined` | `2` | Gap between the cursor point and the panel, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | `true` | Clicking outside the panel closes it.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted, toggled with `v-show`, so you can own the enter/exit animation.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element.
`scrollFade` | `boolean \| undefined` | `true` | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`ui` | `Partial<{ positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the menu is open.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ open: boolean; }` | Wrapped content, such as a card, row or image. Right-clicking it, or long-pressing it on touch, opens the menu.
`header` | `any` | Forwarded to Menu's own `#header`.
`item` | `{ item: T; }` | Override one data-driven row's content while keeping its behavior.
`footer` | `any` | Forwarded to Menu's own `#footer`.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires before the menu closes, with the reason; `details.cancel()` keeps it open.
`select` | `[item: T]` | Fires when a data-driven row is activated, including rows in submenus.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`wrapperEl` | `HTMLElement \| null` | Element wrapping the default slot; listens for right-click and long-press.
`anchorEl` | `HTMLElement \| null` | Zero-size element placed at the open point; the panel anchors to it.
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (null while closed).
`listEl` | `HTMLElement \| null` | Item list element (null while closed).
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`openAt` | `(x: number, y: number) => Promise<void>` | Opens the menu at a viewport point, for example from a custom "⋮" button instead of a right-click.
`close` | `() => void \| undefined` | Closes the menu, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void \| undefined` | Cancels a close pending in `beforeClose` and keeps the menu open.

