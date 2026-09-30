# SplitButton

A primary action fused with a chevron that opens alternate actions in a dropdown.

```ts
import { SplitButton } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly MenuEntry<T>[]` |  | Dropdown rows, in the same shape as Menu's `items`.
`variant` | `ButtonVariant \| undefined` | `'primary'` | Visual style of both buttons.
`size` | `ButtonSize \| undefined` | `'md'` | Size of both buttons.
`disabled` | `boolean \| undefined` | `false` | Disables both buttons and blocks interaction.
`loading` | `boolean \| "auto" \| undefined` | `'auto'` | Forwarded to the main action Button only.
`triggerLabel` | `string \| undefined` |  | `aria-label` for the chevron button; uses the localized "More actions" when unset.
`side` | `Side \| undefined` |  | Which side of the trigger the panel opens on.
`align` | `Align \| undefined` |  | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` |  | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` |  | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` |  | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` |  | Clicking outside the panel closes it.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing.
`forceMount` | `boolean \| undefined` |  | Keeps the panel mounted (toggled with `v-show`) and skips the built-in transition, for JS animation libraries.
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

