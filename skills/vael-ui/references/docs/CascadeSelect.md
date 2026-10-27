# CascadeSelect

A dropdown for picking a path through nested options, one level at a time. `filter` adds a search over every path. Each level's panel has `data-level` (`1`, `2`, …), so `ui.panel` plus `[data-level='2']` styles one level.

```ts
import { CascadeSelect } from 'vael-ui' // or 'vael-ui/vapor'
```

## Notes

`multiple` works like Select's: `v-model` becomes an array of leaf values, picking a leaf toggles it and keeps the panel open on the same path, and branches only ever open their level. Leaves render as `role="menuitemcheckbox"` rows with a check mark. The trigger shows removable chips by default; `maxLabels` collapses the rest into "+N", and `display="text"` / `display="count"` swap chips for comma-joined labels or the `uiKit.cascadeSelect.selectedCount` summary ("{count} selected"). `clearable` empties the array, `name` renders one hidden input per value, and the `#value` slot receives arrays of `selected` items and their `path`s.

```vue
<Field label="Office cities">
  <CascadeSelect v-model="cities" :items="geography" multiple clearable :max-labels="3" />
</Field>
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Tree of options; only leaves (nodes without `children`) can be selected.
`placeholder` | `string \| undefined` |  | Text shown in the trigger when nothing is selected.
`multiple` | `boolean \| undefined` | `false` | Selects several leaves: the model becomes an array and the panel stays open on pick.
`maxLabels` | `number \| undefined` |  | `multiple` only: chips shown before the rest collapse into a "+N" chip.
`display` | `"text" \| "count" \| "chip" \| undefined` | `'chip'` | `multiple` only: `'chip'` shows chips, `'text'` joined labels, `'count'` "N selected".
`disabled` | `boolean \| undefined` | `false` | Disables the trigger and keeps the panel from opening. A disabled parent Field also disables it.
`clearable` | `boolean \| undefined` | `false` | Shows a clear button in the trigger while a value is selected.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | Renders hidden inputs so a plain `<form>` post carries the selection.
`side` | `'top' \| 'right' \| 'bottom' \| 'left' \| undefined` | `'bottom'` | Which side of the trigger the panel opens on.
`align` | `'start' \| 'end' \| 'center' \| undefined` | `'start'` | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the panel along the alignment axis, in pixels.
`panelWidth` | `PanelWidth \| undefined` |  | The first level's width. Unset, it fits its rows and is at least as wide as the field, up to 16rem, so the next level opens close by. `'trigger'` makes it at least the field's full width; a width in px or any CSS length (`'20rem'`) sets it. The levels opening beside it keep their own width.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | `true` | Clicking outside the panel closes it.
`beforeClose` | `BeforeClose<PopoverCloseReason> \| undefined` |  | Runs before the panel closes. Return `true` to close it, `false` to keep it open, or a promise of either, like `() => confirmAction(…).result`. The panel stays open while the promise is pending.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted while closed, toggled with `v-show`, so its content keeps its state. vael-ui then leaves its motion to you.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element.
`filter` | `CascadeSelectFilter<T> \| undefined` |  | Adds a search box to the panel. While you type, the levels become one list of the matching leaves, each with its path. `true` matches every word against the whole path, ignoring case and accents; a function matches your way; `false` leaves filtering to you via `v-model:query`.
`filterPlaceholder` | `string \| undefined` | `'Search...'` | Placeholder for the search box.
`scrollFade` | `boolean \| undefined` | `true` | Masks the panel's top/bottom edge as its content scrolls under it, signaling there's more.
`motion` | `CascadeSelectMotion \| undefined` | `true` | How it moves. `false` skips all of it: the menu opening and easing to its new height as you search, the clear button and the chips. An object moves the menu your way, like Menu's: values for the `panel` part, `{ spring }`, or `enter` / `leave` hooks. `chips` does the same for the chips (`{ chips: { initial: { chip: { opacity: 0, y: 4 } } } }`).
`ui` | `Partial<{ trigger: UiPartValue; value: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; filter: UiPartValue; footer: UiPartValue; start: UiPartValue; end: UiPartValue; clear: UiPartValue; chevron: UiPartValue; filterIcon: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `V \| undefined` | `null` | Selected leaf value, or an array of leaf values with `multiple`.
`open` | `boolean \| undefined` | `false` | Whether the panel is open.
`query` | `string \| undefined` | `''` | Search box text. Clears when the panel closes.

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Content at the start of the trigger, such as an icon, before the value.
`end` | `any` | Content before the built-in clear button and chevron.
`value` | `{ selected: V extends unknown[] ? T[] : T \| null; path: V extends unknown[] ? CascadeSelectPath[] : CascadeSelectPath; }` | Custom trigger content; receives the selected leaf and its path, or arrays of both.
`header` | `any` | Content above the row list, forwarded to the underlying Menu's `#header`.
`item` | `{ item: T; hasChildren: boolean; path: T[]; }` | Custom row content at any level; the row keeps its expand and select behavior. `path` is the items above it, so `path.length` is how deep it sits, even in search results.
`empty` | `any` | Replaces the localized "no options" row shown when `items` is empty.
`footer` | `any` | Content below the row list, forwarded to the underlying Menu's `#footer`.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires before the panel closes, with the reason; `details.cancel()` keeps it open.
`select` | `[item: T, path: CascadeSelectPath]` | Fires when a leaf is picked or unpicked, with the item and its root-to-leaf path.
`change` | `[value: V]` | Fires when you change the selection: picking a leaf, removing a chip, or clearing.
`update:modelValue` | `[value: V \| undefined]` | Fires when `modelValue` changes (`v-model`).
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:query` | `[value: string]` | Fires when `query` changes (`v-model:query`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`triggerEl` | `HTMLElement \| null` | Trigger element: the frame around the button and its clear button. Focusing it focuses the button.
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (null while closed).
`listEl` | `HTMLElement \| null` | Top-level row list element (null while closed).
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`selectedItem` | `T \| null` | Selected leaf item, or `null`; always `null` with `multiple`.
`selectedPath` | `CascadeSelectPath` | Root-to-leaf `value` path of the selection; empty when nothing is selected or with `multiple`.
`open` | `() => void` | Opens the panel.
`close` | `() => void \| undefined` | Closes the panel, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void \| undefined` | Cancels a close pending in `beforeClose` and keeps the panel open.

## Motion

`motion` takes `CascadeSelectMotion` (exported from `'vael-ui'`); `false` turns it off. CascadeSelect's menu, like Menu's. `chips` moves the chips; `valueSwap: false` shows a picked value without its crossfade.

- Parts: `positioner`, `panel`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `enter`, `leave`, `targets`, `morph`, `exitOrder`, `resize`, `onAfterEnter`, `onAfterLeave`
- `chips` (`ChipsMotion`): A select's chips (`multiple`): values or hooks for `chip`, and the list's `stagger` and `leaving`.

See the [Motion guide](../guides/motion.md) for what each key does.

