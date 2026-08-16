# Dial

A circular drag control for picking a numeric value, like a volume knob.

```ts
import { Dial } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` | undefined | Omit either (or both) for a genuinely unbounded value that keeps counting forever in that direction.
`max` | `number \| undefined` | undefined | 
`step` | `number \| undefined` | 1 | 
`degreesPerStep` | `number \| undefined` | undefined | Degrees of pointer rotation per `step` of value change.
`showValue` | `boolean \| undefined` | true | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`disabled` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`name` | `string \| undefined` |  | Falls through to a hidden `<input>` → plain `<form>` participation.
`valueText` | `((value: number) => string) \| undefined` |  | Drives `aria-valuetext`, e.g. `(v) => \`${v} dB\`` for a gain dial.
`ui` | `Partial<{ root: UiPartValue; dial: UiPartValue; track: UiPartValue; fill: UiPartValue; ticks: UiPartValue; face: UiPartValue; }> \| undefined` |  | 
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
`ticksEl` | `SVGGElement \| null` | 

