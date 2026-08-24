# Select

A dropdown for picking one or more values from a list, with search and virtualization.

```ts
import { Select } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | 
`placeholder` | `string \| undefined` |  | 
`multiple` | `boolean \| undefined` | false | 
`disabled` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`loading` | `boolean \| undefined` | false | 
`clearable` | `boolean \| undefined` | false | 
`maxLabels` | `number \| undefined` |  | `multiple` only: how many selected items render as chips before collapsing the rest into a "+N" indicator. Default: uncollapsed.
`display` | `"text" \| "count" \| "chip" \| undefined` | "chip" | `multiple` only: how the trigger renders multiple selections. `'chip'` (default) shows removable chips; `'text'` shows comma-joined labels; `'count'` shows a "N selected" summary. Single-select ignores this prop.
`virtualize` | `boolean \| SelectVirtualizeConfig \| undefined` | undefined | `true`/`false` forces virtualization on/off; an object also tunes `itemSize`/`overscan`. Default: auto-virtualizes past 100 items.
`name` | `string \| undefined` |  | Renders hidden `<input>`(s) so a plain `<form>` post still carries the selection — repeated `name` when `multiple`.
`side` | `Side \| undefined` | "bottom" | 
`align` | `Align \| undefined` | "start" | 
`sideOffset` | `number \| undefined` | 8 | 
`alignOffset` | `number \| undefined` | 0 | 
`closeOnEsc` | `boolean \| undefined` | true | 
`closeOnOutside` | `boolean \| undefined` | true | 
`beforeClose` | `((done: () => void) => void) \| undefined` |  | 
`forceMount` | `boolean \| undefined` | false | 
`teleportTo` | `string \| HTMLElement \| undefined` | "body" | 
`scrollFade` | `boolean \| undefined` | true | 
`motionCss` | `boolean \| undefined` | true | Gates the built-in chip enter/exit/reposition transition (`multiple` + `display="chip"` only). `false` skips it entirely — reach for `@chip-enter`/`@chip-leave` instead if you want a consumer-owned animation (GSAP, motion-v) in its place.
`filter` | `SelectFilter<T> \| undefined` |  | Shows a built-in search box at the top of the panel. `undefined` (default): no box — most lists are short enough that one is just noise. `true`: box + built-in diacritic/case- insensitive label match against `items`. A function: box + your own sync match against the same `items`. `false`: box, but Select does no matching of its own — pair with `v-model:query` and swap `items` yourself (debounced API search, server-side paging). Virtualization already reacts to whatever `items` ends up being, so a remote result set re-virtualizes for free.
`filterPlaceholder` | `string \| undefined` | "Search..." | 
`ui` | `Partial<{ trigger: UiPartValue; value: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; filter: UiPartValue; list: UiPartValue; option: UiPartValue; empty: UiPartValue; footer: UiPartValue; }> \| undefined` |  | 
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | null | 
`query` | `string \| undefined` | "" | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`value` | `{ selected: T \| T[] \| null; }` | 
`header` | `{ count: number; total: number; }` | Above the filter input (if `filter` is on) or the listbox itself. `count`/`total` are handed through for a result-count readout, but the slot is arbitrary content, not just that.
`filter` | `{ query: string; onKeydown: (event: KeyboardEvent) => void; }` | Replaces the built-in filter row entirely — bind your own control straight to `v-model:query` on `<Select>` itself (no need to round-trip through this slot's props for that); `onKeydown` is handed through only so a fully custom input can still opt into arrow/Home/End/Enter listbox navigation the same way the built-in one does.
`filter-icon` | `any` | Swaps just the built-in filter row's leading icon, keeping its `Input` frame.
`item` | `{ item: T; active: boolean; selected: boolean; }` | 
`empty` | `any` | 
`footer` | `any` | Below the listbox — e.g. a "create new" or "view all" action.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`select` | `[item: T]` | 
`change` | `[value: string \| number \| (string \| number)[] \| null]` | 
`reach-end` | `[]` | 
`chip-enter` | `[el: Element, done: () => void]` | 
`chip-leave` | `[el: Element, done: () => void]` | 
`update:open` | `[value: boolean]` | 
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | 
`update:query` | `[value: string]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`triggerEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`panelEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`positionerEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`listEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`filterInputRef` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`placement` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`positionerStyle` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`isClosing` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`open` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`close` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`cancelClose` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`activeIndex` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`scrollToIndex` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.

