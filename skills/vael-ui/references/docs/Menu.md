# Menu

A dropdown list of actions opened from a trigger, with full keyboard navigation.

```ts
import { Menu } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly MenuEntry<T>[] \| undefined` |  | Data-driven rows. Ignored when the default slot renders custom markup instead.
`triggerEl` | `TriggerRef` |  | External ref for a trigger that can't live in the `#trigger` slot.
`side` | `Side \| undefined` | `'bottom'` | Which side of the trigger the panel opens on.
`align` | `Align \| undefined` | `'start'` | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | `true` | Clicking outside the panel closes it.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted, toggled with `v-show`, so you can own the enter/exit animation.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element.
`scrollFade` | `boolean \| undefined` | `true` | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`maxPanelHeight` | `number \| undefined` |  | Caps the panel's height in pixels; the item list scrolls past it. Unset, only the viewport limits it.
`openPath` | `readonly string[] \| undefined` |  | Row path to reveal on open, as each row's `value` (or `label`): every submenu along it opens and its last row takes focus instead of the first, e.g. to show the current selection.
`ui` | `Partial<{ positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the menu is open.

## Slots

Name | Type | Description
--- | --- | ---
`trigger` | `{ open: boolean; }` | Trigger markup. Menu wires the click and anchors the panel to it, so render only the button.
`header` | `any` | Content above the item list, outside its scroll region; it stays put while `items` scrolls.
`item` | `{ item: T; }` | Override one data-driven row's content while keeping its behavior.
`default` | `{ close: () => void; open: boolean; isClosing: boolean; cancelClose: () => void; panelEl: HTMLElement \| null; placement: string; maxHeight: number \| null; }` | Fully custom menu content. Render your own `role="menuitem"` markup; Menu ignores `items`.
`footer` | `any` | Content below the item list, outside its scroll region; it stays put while `items` scrolls.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires before the menu closes, with the reason; `details.cancel()` keeps it open.
`select` | `[item: T]` | Fires when a data-driven row is activated, including rows in submenus.
`collapse` | `[]` | Fires on ArrowLeft inside the menu; a parent menu uses it to close this submenu.
`active` | `[value: boolean]` | Fires when the pointer enters or leaves any submenu row or panel in this subtree; a parent menu uses it to stay open.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (null while closed).
`listEl` | `HTMLElement \| null` | Item list element (null while closed).
`placement` | `Placement` | Resolved placement after flipping, e.g. `'bottom-start'`.
`positionerStyle` | `Record<string, string>` | Inline positioning style applied to the positioner.
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`open` | `() => void` | Opens the menu.
`toggle` | `() => void` | Opens or closes the menu.
`close` | `() => void` | Closes the menu, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the menu open.

