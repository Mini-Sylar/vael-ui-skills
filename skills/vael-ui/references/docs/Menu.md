# Menu

A dropdown list of actions opened from a trigger, with full keyboard navigation. Submenu panels share the `ui` prop and carry `data-level` (`1`, `2`, …) for styling one level.

```ts
import { Menu } from 'vael-ui' // or 'vael-ui/vapor'
```

## Notes

Set `checked` on a `MenuItemData` row to make it a toggle: any boolean renders it as `role="menuitemcheckbox"` with `aria-checked`, and `true` adds a trailing check mark (outside the `#item` slot, so custom rows keep it). Menu doesn't flip the value; update it in `@select` and add `keepOpen` to toggle several in a row. Rows without `checked` render as plain `role="menuitem"` actions.

```ts
const items = ref([{ label: 'Bold', value: 'bold', checked: true, keepOpen: true }])
function onSelect(item) { item.checked = !item.checked }
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly MenuEntry<T>[] \| undefined` |  | Data-driven rows. Ignored when the default slot renders custom markup instead.
`triggerEl` | `TriggerRef` |  | External ref for a trigger that can't live in the `#trigger` slot.
`side` | `'top' \| 'right' \| 'bottom' \| 'left' \| undefined` | `'bottom'` | Which side of the trigger the panel opens on.
`align` | `'start' \| 'end' \| 'center' \| undefined` | `'start'` | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | `true` | Clicking outside the panel closes it.
`beforeClose` | `BeforeClose<PopoverCloseReason> \| undefined` |  | Runs before the menu closes. Return `true` to close it, `false` to keep it open, or a promise of either, like `() => confirmAction(…).result`. The menu stays open while the promise is pending.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted while closed, toggled with `v-show`, so its content keeps its state. vael-ui then leaves its motion to you.
`motion` | `MenuMotion \| undefined` |  | How it moves: `false` for instantly, `{ spring }` to retime the built-in motion, your own `initial` and `exit` for the panel, or `targets` for the rows (`select: '.ui-menu-item'`). Unset, it follows the nearest ConfigProvider's style.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element.
`scrollFade` | `boolean \| undefined` | `true` | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`maxPanelHeight` | `number \| undefined` |  | Caps the panel's height in pixels; the item list scrolls past it. Unset, only the viewport limits it.
`openPath` | `readonly string[] \| undefined` |  | Row path to reveal on open, as each row's `value` (or `label`): every submenu along it opens and its last row takes focus instead of the first, e.g. to show the current selection.
`ui` | `Partial<{ positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; footer: UiPartValue; icon: UiPartValue; chevron: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
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
`active` | `[value: boolean]` | Fires when the pointer enters or leaves any submenu row or panel in this subtree; a parent menu uses it to stay open.
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires before the menu closes, with the reason; `details.cancel()` keeps it open.
`select` | `[item: T]` | Fires when a data-driven row is activated, including rows in submenus.
`collapse` | `[]` | Fires on ArrowLeft inside the menu (ArrowRight right to left); a parent menu uses it to close this submenu.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (null while closed).
`listEl` | `HTMLElement \| null` | Item list element (null while closed).
`highlightEl` | `HTMLElement \| null` | The hover and keyboard highlight, sliding between rows (null while closed). Under a ConfigProvider with `motion: false` it moves at once, for your own animation.
`placement` | `Placement` | Resolved placement after flipping, e.g. `'bottom-start'`.
`positionerStyle` | `Record<string, string>` | Inline positioning style applied to the positioner.
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`open` | `() => void` | Opens the menu.
`toggle` | `() => void` | Opens or closes the menu.
`close` | `() => void` | Closes the menu, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the menu open.

## Motion

`motion` takes `MenuMotion` (exported from `'vael-ui'`); `false` turns it off. Menu's `motion`, and ContextMenu's and SplitButton's: values or hooks for `positioner` and `panel`, and how it opens and closes.

- Parts: `positioner`, `panel`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `targets`, `morph`, `exitOrder`, `resize`, `onAfterEnter`, `onAfterLeave`

See the [Motion guide](../guides/motion.md) for what each key does.

