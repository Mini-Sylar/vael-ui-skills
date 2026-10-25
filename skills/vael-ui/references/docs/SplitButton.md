# SplitButton

A primary action fused with a chevron that opens alternate actions in a dropdown.

```ts
import { SplitButton } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly MenuEntry<T>[]` |  | Dropdown rows, in the same shape as Menu's `items`.
`variant` | `'primary' \| 'secondary' \| 'outline' \| 'ghost' \| 'link' \| 'text' \| 'danger' \| undefined` | `'primary'` | Visual style of both buttons.
`size` | `'sm' \| 'md' \| 'lg' \| undefined` | `'md'` | Size of both buttons.
`unified` | `boolean \| undefined` | `false` | Joins the two buttons into one surface, without the thin line between them.
`disabled` | `boolean \| undefined` | `false` | Disables both buttons and blocks interaction.
`loading` | `boolean \| "auto" \| undefined` | `'auto'` | Forwarded to the main action Button only.
`triggerLabel` | `string \| undefined` |  | `aria-label` for the chevron button; uses the localized "More actions" when unset.
`side` | `'top' \| 'right' \| 'bottom' \| 'left' \| undefined` |  | Which side of the trigger the panel opens on.
`align` | `'start' \| 'end' \| 'center' \| undefined` |  | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` |  | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` |  | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` |  | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` |  | Clicking outside the panel closes it.
`beforeClose` | `BeforeClose<PopoverCloseReason> \| undefined` |  | Runs before the menu closes. Return `true` to close it, `false` to keep it open, or a promise of either, like `() => confirmAction(…).result`. The menu stays open while the promise is pending.
`forceMount` | `boolean \| undefined` |  | Keeps the panel mounted while closed, toggled with `v-show`, so its content keeps its state. vael-ui then leaves its motion to you.
`motion` | `MenuMotion \| undefined` |  | How the menu moves, like Menu's. `false` opens and closes it at once. An object moves it your way: values for the `panel` part, `{ spring }`, or `enter` / `leave` hooks. Unset, it follows the nearest ConfigProvider's style.
`teleportTo` | `string \| HTMLElement \| undefined` |  | Teleport target: a CSS selector or element.
`scrollFade` | `boolean \| undefined` |  | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`ui` | `Partial<{ root: UiPartValue; main: UiPartValue; trigger: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the dropdown menu is open.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Main action button content.
`header` | `any` | Forwarded to the dropdown Menu's own `#header`.
`item` | `{ item: T; }` | Custom content for one dropdown row, keeping its behavior; forwarded to Menu's own `#item`.
`footer` | `any` | Forwarded to the dropdown Menu's own `#footer`.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires before the menu closes; `details` has the `reason` and a `cancel()` that keeps it open.
`select` | `[item: T]` | Fires when a dropdown item is chosen.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`mainEl` | `HTMLElement \| null` | Main action button element.
`triggerEl` | `HTMLElement \| null` | Chevron button element that opens the menu.
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (null while closed).
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`open` | `() => void` | Opens the menu.
`close` | `() => void \| undefined` | Closes the menu, running `beforeClose` first.
`cancelClose` | `() => void \| undefined` | Cancels a close pending in `beforeClose` and keeps the menu open.

## Motion

`motion` takes `MenuMotion` (exported from `'vael-ui'`); `false` turns it off. Menu's `motion`, and ContextMenu's and SplitButton's: values or hooks for `positioner` and `panel`, and how it opens and closes.

- Parts: `positioner`, `panel`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `targets`, `morph`, `exitOrder`, `resize`

See the [Motion guide](../guides/motion.md) for what each key does.

