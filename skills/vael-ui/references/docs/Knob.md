# Knob

A rotary control for picking a numeric value within a fixed arc.

```ts
import { Knob } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` | 0 | 
`max` | `number \| undefined` | 100 | 
`step` | `number \| undefined` | 1 | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`disabled` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`name` | `string \| undefined` |  | Falls through to a hidden `<input>` → plain `<form>` participation.
`valueText` | `((value: number) => string) \| undefined` |  | Drives `aria-valuetext`, e.g. `(v) => \`${v} dB\`` for a gain knob.
`ui` | `Partial<{ root: UiPartValue; dial: UiPartValue; track: UiPartValue; fill: UiPartValue; indicator: UiPartValue; }> \| undefined` |  | 
`modelValue` | `number \| undefined` | 0 | 

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: number]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`dialEl` | `HTMLElement \| null` | 
`indicatorEl` | `HTMLElement \| null` | 

