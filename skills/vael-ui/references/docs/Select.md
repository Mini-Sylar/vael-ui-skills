# Select

A dropdown for picking one or more values from a list, with search, option groups and virtualization.

```ts
import { Select } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly (T \| SelectGroupData<T>)[]` |  | Options to choose from. Put `{ label, items }` entries among them to show options under a group label, like `<optgroup>`.
`placeholder` | `string \| undefined` |  | Text shown in the trigger while nothing is selected.
`multiple` | `boolean \| undefined` | `false` | Lets you select several items. The model becomes an array and the panel stays open on pick.
`disabled` | `boolean \| undefined` | `false` | Disables the trigger and blocks interaction.
`invalid` | `boolean \| undefined` | `false` | Marks the field invalid. ORed with the nearest Field's `error` state.
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Trigger size.
`loading` | `boolean \| undefined` | `false` | Shows a spinner in place of an empty list, or below the rows while more load.
`clearable` | `boolean \| undefined` | `false` | Shows a clear button once something is selected.
`maxLabels` | `number \| undefined` |  | `multiple` only: how many chips show before the rest collapse into a "+N" chip. Unset, all show.
`display` | `"text" \| "count" \| "chip" \| undefined` | `'chip'` | `multiple` only: `'chip'` shows removable chips, `'text'` comma-joined labels, `'count'` an "N selected" summary.
`virtualize` | `boolean \| SelectVirtualizeConfig \| undefined` |  | `true`/`false` forces virtualization on/off; an object also tunes `itemSize`/`overscan`. Unset, it auto-virtualizes past 100 items.
`name` | `string \| undefined` |  | Renders hidden `<input>`(s) so a plain `<form>` post carries the selection, repeating `name` when `multiple`.
`side` | `'top' \| 'right' \| 'bottom' \| 'left' \| undefined` | `'bottom'` | Which side of the trigger the panel opens on.
`align` | `'start' \| 'end' \| 'center' \| undefined` | `'start'` | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | `true` | Clicking outside the panel closes it.
`beforeClose` | `BeforeClose<PopoverCloseReason> \| undefined` |  | Runs before the panel closes. Return `true` to close it, `false` to keep it open, or a promise of either, like `() => confirmAction(…).result`. The panel stays open while the promise is pending.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted while closed, toggled with `v-show`, so its content keeps its state. vael-ui then leaves its motion to you.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element.
`scrollFade` | `boolean \| undefined` | `true` | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`maxPanelHeight` | `number \| undefined` | `320` | Caps the panel height in pixels; the list scrolls past it. The viewport limits it too, so pass `Infinity` to fill the available space.
`motion` | `SelectMotion \| undefined` | `true` | How it moves. `false` skips all of it: the panel opening, the list easing to its new height as you search, the clear button and the chips. An object moves the panel your way, like Popover's: values for the `panel` part, `{ spring }`, or `enter` / `leave` hooks. `chips` does the same for the chips (`{ chips: { initial: { chip: { opacity: 0, y: 4 } } } }`).
`filter` | `SelectFilter<T> \| undefined` |  | Adds a search box to the panel. `true` matches labels ignoring case and accents, a function matches your way, `false` leaves filtering to you via `v-model:query`.
`filterPlaceholder` | `string \| undefined` | `'Search...'` | Placeholder for the search box.
`ui` | `Partial<{ trigger: UiPartValue; value: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; filter: UiPartValue; list: UiPartValue; option: UiPartValue; group: UiPartValue; groupLabel: UiPartValue; empty: UiPartValue; footer: UiPartValue; start: UiPartValue; end: UiPartValue; clear: UiPartValue; chevron: UiPartValue; filterIcon: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | `null` | Selected value, or an array of values when `multiple`.
`query` | `string \| undefined` | `''` | Search box text. Clears when the panel closes.
`open` | `boolean \| undefined` | `false` | Whether the panel is open.

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Content at the start of the trigger, such as an icon, before the value.
`end` | `any` | Content before the built-in clear button and chevron.
`value` | `{ selected: T \| T[] \| null; }` | Custom trigger content for the current selection (an array when `multiple`). It replaces the built-in label and its crossfade; to keep the crossfade, put the text in `<AnimatedText :value="…" />`.
`header` | `{ count: number; total: number; }` | Content above the filter input (when `filter` is on) or the listbox. `count` and `total` support a result-count readout.
`filter` | `{ query: string; onKeydown: (event: KeyboardEvent) => void; }` | Replaces the built-in filter row. Bind your input to `v-model:query`; pass keys to `onKeydown` to keep arrow/Home/End/Enter list navigation.
`filter-icon` | `any` | Replaces only the built-in filter row's leading icon and keeps its Input frame.
`item` | `{ item: T; index: number; active: boolean; selected: boolean; group?: SelectGroupSlotData<T> \| undefined; }` | Custom row content for each option. `group` is the group it sits in, if any.
`group-label` | `{ group: SelectGroupSlotData<T>; items: T[]; }` | Custom content for a group's label. `group` is the object you passed, with your own fields; `items` are its options still showing. Screen readers read this content as the group's name.
`empty` | `any` | Replaces the text shown when no items match.
`footer` | `any` | Content below the listbox, such as a "create new" or "view all" action.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires before the panel closes; `details.cancel()` keeps it open.
`select` | `[item: T]` | Fires when you pick an item, including a pick that deselects it in `multiple` mode.
`change` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when you change the selection, including chip removal and clear.
`reach-end` | `[]` | Fires when the list's last rows render, for loading more. Re-arms when the item count changes.
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when `modelValue` changes (`v-model`).
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:query` | `[value: string]` | Fires when `query` changes (`v-model:query`).

## Exposed

Name | Type | Description
--- | --- | ---
`triggerEl` | `HTMLElement \| null` | Trigger element: the frame around the combobox and its clear button. Focusing it focuses the combobox.
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (null while closed).
`listEl` | `HTMLElement \| null` | Scrollable listbox element (null while closed).
`highlightEl` | `HTMLElement \| null` | The hover and keyboard highlight, sliding between options (null while closed). With `motion: false` it moves at once, for your own animation.
`filterInputRef` | `{ el: HTMLElement \| null; inputEl: HTMLInputElement \| null; } \| null` | Built-in search Input instance (null without `filter`, with a `#filter` slot, or while closed).
`placement` | `Placement` | Resolved placement after flipping, e.g. `'bottom-start'`.
`positionerStyle` | `Record<string, string>` | Inline positioning styles applied to the positioner.
`isClosing` | `boolean` | True while a `beforeClose` close is pending.
`open` | `() => void` | Opens the panel (no-op while disabled).
`close` | `() => void` | Closes the panel, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the panel open.
`activeIndex` | `number` | Index of the highlighted row in the filtered list (-1 for none).
`scrollToIndex` | `(index: number, align?: ScrollAlign \| undefined) => void` | Scrolls the list to the row at `index`.

## Motion

`motion` takes `SelectMotion` (exported from `'vael-ui'`); `false` turns it off. Select's panel: values or hooks for its parts, like Popover's. `chips` moves the chips.

- Parts: `positioner`, `panel`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `targets`, `morph`, `exitOrder`, `resize`, `onAfterEnter`, `onAfterLeave`
- `chips` (`ChipsMotion`): A select's chips (`multiple`): values or hooks for `chip`, and the list's `stagger` and `leaving`.

See the [Motion guide](../guides/motion.md) for what each key does.

