# Select

A dropdown for picking one or more values from a list, with search and virtualization.

```ts
import { Select } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Options to choose from.
`placeholder` | `string \| undefined` |  | Text shown in the trigger while nothing is selected.
`multiple` | `boolean \| undefined` | `false` | Lets you select several items. The model becomes an array and the panel stays open on pick.
`disabled` | `boolean \| undefined` | `false` | Disables the trigger and blocks interaction.
`invalid` | `boolean \| undefined` | `false` | Marks the field invalid. ORed with the nearest Field's `error` state.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Trigger size.
`loading` | `boolean \| undefined` | `false` | Shows a spinner in place of an empty list, or below the rows while more load.
`clearable` | `boolean \| undefined` | `false` | Shows a clear button once something is selected.
`maxLabels` | `number \| undefined` |  | `multiple` only: how many chips show before the rest collapse into a "+N" chip. Unset, all show.
`display` | `"text" \| "count" \| "chip" \| undefined` | `'chip'` | `multiple` only: `'chip'` shows removable chips, `'text'` comma-joined labels, `'count'` an "N selected" summary.
`virtualize` | `boolean \| SelectVirtualizeConfig \| undefined` |  | `true`/`false` forces virtualization on/off; an object also tunes `itemSize`/`overscan`. Unset, it auto-virtualizes past 100 items.
`name` | `string \| undefined` |  | Renders hidden `<input>`(s) so a plain `<form>` post carries the selection, repeating `name` when `multiple`.
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
`maxPanelHeight` | `number \| undefined` | `320` | Caps the panel height in pixels; the list scrolls past it. The viewport limits it too, so pass `Infinity` to fill the available space.
`motionCss` | `boolean \| undefined` | `true` | `false` skips the built-in chip transitions (`multiple` only); animate them yourself via `@chip-enter`/`@chip-leave`.
`filter` | `SelectFilter<T> \| undefined` |  | Adds a search box to the panel. `true` matches labels ignoring case and accents, a function matches your way, `false` leaves filtering to you via `v-model:query`.
`filterPlaceholder` | `string \| undefined` | `'Search...'` | Placeholder for the search box.
`ui` | `Partial<{ trigger: UiPartValue; value: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; filter: UiPartValue; list: UiPartValue; option: UiPartValue; empty: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | `null` | Selected value, or an array of values when `multiple`.
`query` | `string \| undefined` | `''` | Search box text. Clears when the panel closes.
`open` | `boolean \| undefined` | `false` | Whether the panel is open.

## Slots

Name | Type | Description
--- | --- | ---
`value` | `{ selected: T \| T[] \| null; }` | Custom trigger content for the current selection (an array when `multiple`).
`header` | `{ count: number; total: number; }` | Content above the filter input (when `filter` is on) or the listbox. `count` and `total` support a result-count readout.
`filter` | `{ query: string; onKeydown: (event: KeyboardEvent) => void; }` | Replaces the built-in filter row. Bind your input to `v-model:query`; pass keys to `onKeydown` to keep arrow/Home/End/Enter list navigation.
`filter-icon` | `any` | Replaces only the built-in filter row's leading icon and keeps its Input frame.
`item` | `{ item: T; active: boolean; selected: boolean; }` | Custom row content for each option.
`empty` | `any` | Replaces the text shown when no items match.
`footer` | `any` | Content below the listbox, such as a "create new" or "view all" action.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires before the panel closes; `details.cancel()` keeps it open.
`select` | `[item: T]` | Fires when you pick an item, including a pick that deselects it in `multiple` mode.
`change` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when you change the selection, including chip removal and clear.
`reach-end` | `[]` | Fires when the list's last rows render, for loading more. Re-arms when the item count changes.
`chip-enter` | `[el: Element, done: () => void]` | Fires instead of the built-in CSS transition when `motionCss` is `false`. Call `done()` once your enter animation finishes.
`chip-leave` | `[el: Element, done: () => void]` | Same as `@chip-enter`, for a chip's removal.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when `modelValue` changes (`v-model`).
`update:query` | `[value: string]` | Fires when `query` changes (`v-model:query`).

## Exposed

Name | Type | Description
--- | --- | ---
`triggerEl` | `HTMLElement \| null` | Trigger element.
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (null while closed).
`listEl` | `HTMLElement \| null` | Scrollable listbox element (null while closed).
`filterInputRef` | `{ el: HTMLElement \| null; inputEl: HTMLInputElement \| null; } \| null` | Built-in search Input instance (null without `filter`, with a `#filter` slot, or while closed).
`placement` | `Placement` | Resolved placement after flipping, e.g. `'bottom-start'`.
`positionerStyle` | `Record<string, string>` | Inline positioning styles applied to the positioner.
`isClosing` | `boolean` | True while a `beforeClose` close is pending.
`open` | `() => void` | Opens the panel (no-op while disabled).
`close` | `() => void` | Closes the panel, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the panel open.
`activeIndex` | `number` | Index of the highlighted row in the filtered list (-1 for none).
`scrollToIndex` | `(index: number, align?: ScrollAlign \| undefined) => void` | Scrolls the list to the row at `index`.

