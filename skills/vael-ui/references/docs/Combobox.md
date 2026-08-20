# Combobox

A searchable dropdown that also accepts free text, so autocomplete and tagging are one component.

```ts
import { Combobox } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | 
`placeholder` | `string \| undefined` |  | 
`multiple` | `boolean \| undefined` | false | Model becomes `(string \| number)[]`. Selecting adds an item without closing the panel; clicking a selected row removes it. Selections render as removable chips.
`maxLabels` | `number \| undefined` |  | `multiple` only: how many selected items render as chips before collapsing the rest into a "+N" indicator. Default: uncollapsed.
`disabled` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`loading` | `boolean \| undefined` | false | 
`clearable` | `boolean \| undefined` | false | 
`virtualize` | `boolean \| SelectVirtualizeConfig \| undefined` | undefined | `true`/`false` forces virtualization on/off; an object also tunes `itemSize`/`overscan`. Default: auto-virtualizes past 100 items.
`name` | `string \| undefined` |  | 
`filter` | `ComboboxFilter<T> \| undefined` | true | `true` (default): case/diacritic-insensitive local match on `getLabel`. `false`: consumer filters (bind `items` to an async result) — `filteredItems` becomes `items` verbatim. A function: fully custom local match.
`allowCustom` | `boolean \| undefined` | false | Enter with no active option, or blur with unmatched text, commits the raw typed string as the model value and emits `create`.
`openOnFocus` | `boolean \| undefined` | undefined | Opens the panel on focus before typing (the discoverability default). Set `false` to require typing first.
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
`motionCss` | `boolean \| undefined` | true | Gates the built-in chip enter/exit/reposition transition (`multiple` only). `false` skips it entirely — reach for `@chip-enter`/`@chip-leave` instead if you want a consumer-owned animation (GSAP, motion-v) in its place.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; positioner: UiPartValue; panel: UiPartValue; list: UiPartValue; option: UiPartValue; empty: UiPartValue; }> \| undefined` |  | 
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | null | 
`query` | `string \| undefined` | "" | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | 
`end` | `any` | Renders before the library's own clear/chevron, inside Input's `#end`.
`item` | `{ item: T; active: boolean; selected: boolean; }` | 
`empty` | `any` | 

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`select` | `[item: T]` | 
`change` | `[value: string \| number \| (string \| number)[] \| null]` | 
`reach-end` | `[]` | 
`chip-enter` | `[el: Element, done: () => void]` | 
`chip-leave` | `[el: Element, done: () => void]` | 
`create` | `[query: string]` | 
`update:open` | `[value: boolean]` | 
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | 
`update:query` | `[value: string]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`inputEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`panelEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`positionerEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`listEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`placement` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`positionerStyle` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`isClosing` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`open` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`close` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`cancelClose` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`activeIndex` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`scrollToIndex` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.

