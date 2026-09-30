# Knob

A rotary control for picking a numeric value within a fixed arc.

```ts
import { Knob } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` | `0` | Lowest allowed value.
`max` | `number \| undefined` | `100` | Highest allowed value.
`step` | `number \| undefined` | `1` | Increment the value snaps to; arrow keys move one step, Page Up/Down ten.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Control size.
`disabled` | `boolean \| undefined` | `false` | Disables the knob and blocks interaction.
`invalid` | `boolean \| undefined` | `false` | Shows the invalid state. A surrounding `Field` in error sets it too.
`name` | `string \| undefined` |  | Native `name` for form submission, via a hidden input.
`valueText` | `((value: number) => string) \| undefined` |  | Drives `aria-valuetext`, e.g. `(v) => \`${v} dB\`` for a gain knob.
`ui` | `Partial<{ root: UiPartValue; dial: UiPartValue; track: UiPartValue; fill: UiPartValue; indicator: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
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
`indicatorEl` | `HTMLElement \| null` | Pointer mark that rotates with the value. 

