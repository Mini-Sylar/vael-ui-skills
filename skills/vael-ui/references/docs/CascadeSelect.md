# CascadeSelect

A dropdown for picking a path through nested options, one level at a time.

```ts
import { CascadeSelect } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | 
`placeholder` | `string \| undefined` | undefined | 
`disabled` | `boolean \| undefined` | false | 
`clearable` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | Standalone override; ORed with the nearest Field's `error` state.
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`name` | `string \| undefined` | undefined | Renders a hidden `<input type="hidden">` mirroring the leaf value, for plain `<form>` posts.
`side` | `Side \| undefined` | "bottom" | 
`align` | `Align \| undefined` | "start" | 
`sideOffset` | `number \| undefined` | 8 | 
`alignOffset` | `number \| undefined` | 0 | 
`closeOnEsc` | `boolean \| undefined` | true | 
`closeOnOutside` | `boolean \| undefined` | true | 
`beforeClose` | `((done: () => void) => void) \| undefined` | undefined | 
`forceMount` | `boolean \| undefined` | false | 
`teleportTo` | `string \| HTMLElement \| undefined` | "body" | 
`scrollFade` | `boolean \| undefined` | true | 
`ui` | `Partial<{ trigger: UiPartValue; value: UiPartValue; positioner: UiPartValue; panel: UiPartValue; }> \| undefined` | undefined | 
`modelValue` | `string \| number \| null \| undefined` | null | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`value` | `{ selected: T \| null; path: CascadeSelectPath; }` | Trigger content override — receives the resolved leaf item and its root-to-leaf path.
`item` | `{ item: T; hasChildren: boolean; }` | Row content override, any level — keeps the row's expand/select behavior.
`empty` | `any` | Replaces the localized "no options" row shown when `items` is empty.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`select` | `[item: T, path: CascadeSelectPath]` | 
`change` | `[value: string \| number \| null]` | 
`update:open` | `[value: boolean]` | 
`update:modelValue` | `[value: string \| number \| null]` | 

## Exposed

_None._

