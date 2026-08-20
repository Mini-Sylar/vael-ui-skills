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
`ui` | `Partial<{ trigger: UiPartValue; value: UiPartValue; positioner: UiPartValue; panel: UiPartValue; list: UiPartValue; option: UiPartValue; empty: UiPartValue; }> \| undefined` |  | 
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | null | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`value` | `{ selected: T \| T[] \| null; }` | 
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
`update:open` | `[value: boolean]` | 
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | 

## Exposed

_None._

