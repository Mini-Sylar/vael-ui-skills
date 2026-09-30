# Combobox

A searchable dropdown that also accepts free text, so autocomplete and tagging are one component.

```ts
import { Combobox } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Options to choose from.
`placeholder` | `string \| undefined` |  | Text shown in the input while it's empty.
`multiple` | `boolean \| undefined` | `false` | Lets you select several items. The model becomes an array, the panel stays open on pick, and selections show as removable chips.
`maxLabels` | `number \| undefined` |  | `multiple` only: how many chips show before the rest collapse into a "+N" chip. Unset, all show.
`disabled` | `boolean \| undefined` | `false` | Disables the input and blocks interaction.
`invalid` | `boolean \| undefined` | `false` | Marks the field invalid. ORed with the nearest Field's `error` state.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Input size.
`loading` | `boolean \| undefined` | `false` | Shows a spinner in place of an empty list, or below the rows while more load.
`clearable` | `boolean \| undefined` | `false` | Shows a clear button once there's a selection or typed text; clearing empties both.
`virtualize` | `boolean \| SelectVirtualizeConfig \| undefined` |  | `true`/`false` forces virtualization on/off; an object also tunes `itemSize`/`overscan`. Unset, it auto-virtualizes past 100 items.
`name` | `string \| undefined` |  | Renders hidden `<input>`(s) so a plain `<form>` post carries the selection, repeating `name` when `multiple`.
`filter` | `ComboboxFilter<T> \| undefined` | `true` | How typed text filters `items`. `true` matches labels ignoring case and accents; a function matches your way. `false` shows `items` as-is so you can filter them yourself.
`allowCustom` | `boolean \| undefined` | `false` | Lets typed text that isn't an item become the value, after a cancelable `@create`. Add it to `items` yourself if it should become a real option.
`createOption` | `boolean \| undefined` | `true` | `allowCustom` only: appends a `Create "…"` row whenever the typed text isn't already an item's label. Customize it with `#create`.
`commitOnBlur` | `boolean \| undefined` | `false` | `allowCustom` only: leaving the field with uncommitted text commits it (reason `'blur'`). When `false`, the text reverts instead.
`tabBehavior` | `ComboboxTabBehavior \| undefined` |  | What Tab commits before focus moves on. `'select'`: the highlighted row, once you type or move the highlight with the keyboard. `'create'` (`allowCustom` only): the typed text, or the item it names.
`openOnFocus` | `boolean \| undefined` |  | Opens the panel on focus, before you type. Unset means on; set `false` to require typing first.
`side` | `Side \| undefined` | `'bottom'` | Which side of the input the panel opens on.
`align` | `Align \| undefined` | `'start'` | How the panel aligns against the input along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the input and the panel, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel (and reverts uncommitted typing).
`closeOnOutside` | `boolean \| undefined` | `true` | Clicking outside the panel, or tabbing away, closes it.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted, toggled with `v-show`, so you can own the enter/exit animation.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element.
`scrollFade` | `boolean \| undefined` | `true` | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`maxPanelHeight` | `number \| undefined` | `320` | Caps the panel height in pixels; the list scrolls past it. The viewport limits it too, so pass `Infinity` to fill the available space.
`motionCss` | `boolean \| undefined` | `true` | `false` skips the built-in chip transitions (`multiple` only); animate them yourself via `@chip-enter`/`@chip-leave`.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; list: UiPartValue; option: UiPartValue; empty: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | `null` | Selected value, or an array of values when `multiple`.
`query` | `string \| undefined` | `''` | Input text. Holds the selected label in single mode; clears after each pick when `multiple`.
`open` | `boolean \| undefined` | `false` | Whether the panel is open.

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Content before the input text, after any chips.
`end` | `any` | Content before the built-in clear button and chevron, inside Input's `#end`.
`header` | `{ count: number; total: number; }` | Content above the listbox, inside the popover panel. `count` and `total` support a result-count readout.
`item` | `{ item: T; index: number; active: boolean; selected: boolean; query: string; }` | Custom row content for each option. `query` is the trimmed typed text, for highlighting the match.
`create` | `{ query: string; active: boolean; }` | Content of the `allowCustom` Create row.
`empty` | `any` | Replaces the text shown when no items match.
`footer` | `any` | Content below the listbox, inside the popover panel.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires before the panel closes; `details.cancel()` keeps it open.
`select` | `[item: T]` | Fires when you pick an item, including a pick that deselects it in `multiple` mode.
`change` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when you change the value, including custom values, chip removal and clear.
`reach-end` | `[]` | Fires when the list's last rows render, for loading more. Re-arms when the item count changes.
`chip-enter` | `[el: Element, done: () => void]` | Fires instead of the built-in CSS transition when `motionCss` is `false`. Call `done()` once your enter animation finishes.
`chip-leave` | `[el: Element, done: () => void]` | Same as `@chip-enter`, for a chip's removal.
`create` | `[query: string, details: ComboboxCreateDetails]` | Fires before the typed text commits as a custom value. Call `details.cancel()` to veto it, for validation or an async create that sets the model itself.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when `modelValue` changes (`v-model`).
`update:query` | `[value: string]` | Fires when `query` changes (`v-model:query`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element (the input's frame).
`inputEl` | `HTMLInputElement \| null` | Native `<input>` element.
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (null while closed).
`listEl` | `HTMLElement \| null` | Scrollable listbox element (null while closed).
`placement` | `Placement` | Resolved placement after flipping, e.g. `'bottom-start'`.
`positionerStyle` | `Record<string, string>` | Inline positioning styles applied to the positioner.
`isClosing` | `boolean` | True while a `beforeClose` close is pending.
`open` | `() => void` | Opens the panel (no-op while disabled).
`close` | `() => void` | Closes the panel, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the panel open.
`activeIndex` | `number` | Index of the highlighted row in the list (-1 for none).
`scrollToIndex` | `(index: number, align?: ScrollAlign \| undefined) => void` | Scrolls the list to the row at `index`.

