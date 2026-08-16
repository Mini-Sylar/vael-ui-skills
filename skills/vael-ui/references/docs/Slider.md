# Slider

Drag a handle (or two, for a range) along a track to pick a numeric value.

```ts
import { Slider } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` | 0 | 
`max` | `number \| undefined` | 100 | 
`step` | `number \| undefined` | 1 | 
`orientation` | `"horizontal" \| "vertical" \| undefined` | "horizontal" | 
`disabled` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`name` | `string \| undefined` |  | Falls through to hidden `<input>`(s) → plain `<form>` participation.
`valueText` | `((value: number) => string) \| undefined` |  | Drives `aria-valuetext`, e.g. `(v) => \`$${v}\`` for a currency slider.
`ui` | `Partial<{ root: UiPartValue; track: UiPartValue; fill: UiPartValue; thumb: UiPartValue; }> \| undefined` |  | 
`modelValue` | `number \| [number, number] \| undefined` | 0 | 

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: number \| [number, number]]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`trackEl` | `HTMLElement \| null` | 
`fillEl` | `HTMLElement \| null` | 
`thumbEls` | `HTMLElement[] \| null` | 

