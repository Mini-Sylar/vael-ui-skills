# CascadeSelect

A dropdown for picking a path through nested options, one level at a time.

```ts
import { CascadeSelect } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Tree of options; only leaves (nodes without `children`) can be selected.
`placeholder` | `string \| undefined` |  | Text shown in the trigger when nothing is selected.
`disabled` | `boolean \| undefined` | `false` | Disables the trigger and keeps the panel from opening. A disabled parent Field also disables it.
`clearable` | `boolean \| undefined` | `false` | Shows a clear button in the trigger while a value is selected.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | When set, renders an `<input type="hidden">` with this name that mirrors the leaf value, for plain `<form>` posts.
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
`ui` | `Partial<{ trigger: UiPartValue; value: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| number \| null \| undefined` | `null` | Selected leaf value.
`open` | `boolean \| undefined` | `false` | Whether the panel is open.

## Slots

Name | Type | Description
--- | --- | ---
`value` | `{ selected: T \| null; path: CascadeSelectPath; }` | Custom trigger content; receives the selected leaf item and its root-to-leaf path.
`header` | `any` | Content above the row list, forwarded to the underlying Menu's `#header`.
`item` | `{ item: T; hasChildren: boolean; }` | Custom row content at any level; the row keeps its expand and select behavior.
`empty` | `any` | Replaces the localized "no options" row shown when `items` is empty.
`footer` | `any` | Content below the row list, forwarded to the underlying Menu's `#footer`.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires before the panel closes, with the reason; `details.cancel()` keeps it open.
`select` | `[item: T, path: CascadeSelectPath]` | Fires when a leaf is picked, with the item and its root-to-leaf path.
`change` | `[value: string \| number \| null]` | Fires when the value changes by picking a leaf or clearing (`null`).
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:modelValue` | `[value: string \| number \| null]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`triggerEl` | `HTMLElement \| null` | Trigger element.
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (null while closed).
`listEl` | `HTMLElement \| null` | Top-level row list element (null while closed).
`isClosing` | `boolean` | `true` while a `beforeClose` close is pending.
`selectedItem` | `CascadeSelectItem \| null` | Selected leaf item, or `null`.
`selectedPath` | `CascadeSelectPath` | Root-to-leaf `value` path of the selection (empty when nothing is selected).
`open` | `() => void` | Opens the panel.
`close` | `() => void \| undefined` | Closes the panel, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void \| undefined` | Cancels a close pending in `beforeClose` and keeps the panel open.

