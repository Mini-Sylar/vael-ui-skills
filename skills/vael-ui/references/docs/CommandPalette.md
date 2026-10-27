# CommandPalette

A searchable list of commands in a dialog, opened from a button or a keyboard shortcut such as ⌘K.

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
`size` | `'sm' \| 'md' \| 'lg' \| undefined` | `'lg'` | Panel width, as in Dialog.
`position` | `'top' \| 'right' \| 'bottom' \| 'left' \| 'center' \| undefined` | `'top'` | Where the panel anchors in the viewport. `'top'` matches the Spotlight/Raycast convention; `'center'` feels more like a Dialog.
`modal` | `boolean \| undefined` | `true` | `false` turns off the overlay, scroll lock, focus trap and the inert page behind it. Escape-to-close and layer stacking still apply.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOverlay` | `boolean \| undefined` | `true` | Clicking the overlay closes the panel. Has no effect when `modal` is `false`, since there's no overlay.
`beforeClose` | `BeforeClose<DialogCloseReason> \| undefined` |  | Runs before the palette closes. Return `true` to close it, `false` to keep it open, or a promise of either, like `() => confirmAction(…).result`. The palette stays open while the promise is pending. CommandPalette passes it to its Dialog.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted while closed, toggled with `v-show`, so its content keeps its state. vael-ui then leaves its motion to you. Forwarded to the underlying Dialog.
`motion` | `CommandPaletteMotion \| undefined` | `true` | How it moves. `false` skips all of it: opening, closing, and the results easing to their new height as you type (rows change at once). An object moves the dialog your way, like Dialog's: values for its parts, `{ spring }`, or `enter` / `leave` hooks. `items` animates the rows (`{ items: { initial: { item: { opacity: 0 } } } }`).
`teleportTo` | `string \| undefined` |  | Teleport target: a CSS selector. Takes precedence over `container`.
`container` | `DOMTarget \| undefined` |  | Scopes the palette to one element instead of the viewport. Forwarded to the underlying Dialog.
`scrollTarget` | `DOMTarget \| undefined` |  | Element whose scrolling locks while the palette is open, if different from `container`. Forwarded to the underlying Dialog.
`ui` | `Partial<{ panel: UiPartValue; input: UiPartValue; list: UiPartValue; groupLabel: UiPartValue; item: UiPartValue; empty: UiPartValue; footer: UiPartValue; icon: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the palette is open.
`query` | `string \| undefined` | `''` | Search text. Resets to empty each time the palette opens.

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; index: number; active: boolean; group: string \| undefined; select: () => void; }` | Custom row content for each item; call `select()` to pick it. `group` is its group's name.
`group-label` | `{ group: string; items: T[]; }` | Custom content for a group's heading. `group` is its name; `items` are its items still showing.
`empty` | `any` | Replaces the localized "no results" row shown when nothing matches.
`footer` | `{ activeItem: T \| undefined; query: string; }` | A strip under the list, such as key hints. `activeItem` is the highlighted item, if any.

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
`highlightEl` | `HTMLElement \| null` | The hover and keyboard highlight, sliding between items (null while closed). With `motion: false` it moves at once, for your own animation.
`filteredItems` | `T[]` | Items matching the current query, in `items` order.
`activeIndex` | `number` | Index of the highlighted item in `filteredItems` (`-1` when none).
`activeItem` | `T \| undefined` | The highlighted item, for acting on it from your own keys (copy, open in a new tab).

## Motion

`motion` takes `CommandPaletteMotion` (exported from `'vael-ui'`); `false` turns it off. CommandPalette's dialog, like Dialog's. `items` moves the result rows.

- Parts: `overlay`, `panel`, `header`, `title`, `description`, `body`, `footer`, `maximize`, `close`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `targets`, `morph`, `exitOrder`, `resize`, `onAfterEnter`, `onAfterLeave`, `linked`, `swap`, `direction`, `onAfterSwap`, `items`
- `items` (`CommandPaletteItemsMotion`): The result rows: values or hooks for `item`, and the list's `stagger`.

See the [Motion guide](../guides/motion.md) for what each key does.

