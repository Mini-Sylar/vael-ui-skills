# Dial

A circular drag control for picking a numeric value, like a volume knob.

```ts
import { Dial } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` |  | Lowest allowed value. Omit for no lower bound.
`max` | `number \| undefined` |  | Highest allowed value. Omit for no upper bound; the progress ring shows only when both are set.
`step` | `number \| undefined` | `1` | Value change per arrow key or per `degreesPerStep` of rotation. Page Up/Down move ten steps.
`degreesPerStep` | `number \| undefined` |  | Degrees of pointer rotation per `step` of value change. Unset, uses 15.
`showValue` | `boolean \| undefined` | `true` | Shows the current value in the center of the dial.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Control size.
`disabled` | `boolean \| undefined` | `false` | Disables the dial and blocks interaction.
`invalid` | `boolean \| undefined` | `false` | Shows the invalid state. A surrounding `Field` in error sets it too.
`name` | `string \| undefined` |  | Native `name` for form submission, via a hidden input.
`valueText` | `((value: number) => string) \| undefined` |  | Drives `aria-valuetext`, e.g. `(v) => \`${v} dB\`` for a gain dial.
`ui` | `Partial<{ root: UiPartValue; dial: UiPartValue; track: UiPartValue; fill: UiPartValue; ticks: UiPartValue; face: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `number \| undefined` | `0` | Current value.

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: number]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`dialEl` | `HTMLElement \| null` | Focusable dial element (`role="slider"`).
`ticksEl` | `SVGGElement \| null` | SVG group holding the tick marks. 

