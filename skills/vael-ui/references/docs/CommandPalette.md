# CommandPalette

```ts
import { CommandPalette } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Commands to search and run.
`placeholder` | `string \| undefined` |  | Search input placeholder. Falls back to the localized message.
`filter` | `((item: T, query: string) => boolean) \| undefined` |  | Custom filter function; replaces the default label/keywords match. Omit to use the built-in filter.
`shortcut` | `string \| undefined` |  | Global shortcut that toggles `open`, such as `'mod+k'` (`mod` is Cmd on Mac, Ctrl elsewhere). Nothing listens until you set it.
`closeOnSelect` | `boolean \| undefined` | `true` | Closes the palette after an item is selected.
`size` | `DialogSize \| undefined` | `'lg'` | Panel width, as in Dialog.
`position` | `DialogPosition \| undefined` | `'top'` | Where the panel anchors in the viewport. `'top'` matches the Spotlight/Raycast convention; `'center'` feels more like a Dialog.
`modal` | `boolean \| undefined` | `true` | `false` disables the overlay, scroll lock and focus trap. Escape-to-close and layer stacking still apply.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOverlay` | `boolean \| undefined` | `true` | Clicking the overlay closes the panel. Has no effect when `modal` is `false`, since there's no overlay.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing. Forwarded to the underlying Dialog.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted, toggled with `v-show`, so you can own the enter/exit animation. Forwarded to the underlying Dialog.
`teleportTo` | `string \| undefined` |  | Teleport target: a CSS selector. Takes precedence over `container`.
`container` | `DOMTarget \| undefined` |  | Scopes the palette to one element instead of the viewport. Forwarded to the underlying Dialog.
`scrollTarget` | `DOMTarget \| undefined` |  | Element whose scrolling locks while the palette is open, if different from `container`. Forwarded to the underlying Dialog.
`ui` | `Partial<{ panel: UiPartValue; input: UiPartValue; list: UiPartValue; groupLabel: UiPartValue; item: UiPartValue; empty: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the palette is open.
`query` | `string \| undefined` | `''` | Search text. Resets to empty each time the palette opens.

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; index: number; active: boolean; select: () => void; }` | Custom row content for each item; call `select()` to pick it.
`empty` | `any` | Replaces the localized "no results" row shown when nothing matches.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: DialogOpenChangeDetails]` | Fires before the palette closes, with the reason; `details.cancel()` keeps it open.
`select` | `[item: T]` | Fires when you pick an enabled item by click or Enter.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:query` | `[value: string]` | Fires when `query` changes (`v-model:query`).

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`close` | `() => void \| undefined` | Closes the palette, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void \| undefined` | Cancels a close pending in `beforeClose` and keeps the palette open.
`inputEl` | `HTMLInputElement \| null` | Search input element (null while closed).
`listEl` | `HTMLElement \| null` | Results list element (null while closed).
`filteredItems` | `readonly T[]` | Items matching the current query, in `items` order.
`activeIndex` | `number` | Index of the highlighted item in `filteredItems` (`-1` when none).

