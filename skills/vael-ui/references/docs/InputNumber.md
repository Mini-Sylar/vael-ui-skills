# InputNumber

A numeric field with increment/decrement controls and format-aware parsing.

```ts
import { InputNumber } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` |  | Lowest allowed value; blur, Enter and the steppers clamp the value to it.
`max` | `number \| undefined` |  | Highest allowed value; blur, Enter and the steppers clamp the value to it.
`step` | `number \| undefined` | `1` | Amount the steppers and the arrow keys add or remove.
`locale` | `string \| undefined` |  | BCP-47 locale for formatting and parsing numbers. When unset, the runtime locale applies.
`mode` | `"decimal" \| "currency" \| "percent" \| undefined` | `'decimal'` | Number format: plain decimal, currency (needs `currency`) or percent.
`currency` | `string \| undefined` |  | ISO 4217 currency code; required when `mode` is `'currency'`.
`minFractionDigits` | `number \| undefined` |  | Minimum number of decimal places shown.
`maxFractionDigits` | `number \| undefined` |  | Maximum number of decimal places shown.
`useGrouping` | `boolean \| undefined` | `true` | Shows locale grouping separators, such as thousands commas.
`prefix` | `string \| undefined` |  | Literal text before the formatted number, outside Intl formatting, such as a unit label.
`suffix` | `string \| undefined` |  | Literal text after the formatted number, such as a unit label.
`controls` | `boolean \| undefined` | `true` | Shows the increment and decrement buttons.
`stepperPosition` | `"split" \| "end" \| undefined` | `'end'` | `'end'` stacks a +/- column after the value; `'split'` puts one full-height button on each side.
`allowEmpty` | `boolean \| undefined` | `true` | When `false`, blurring an empty field sets the value to `min ?? 0` instead of `null`.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Control size.
`disabled` | `boolean \| undefined` | `false` | Disables the input and blocks interaction. A disabled parent Field also disables it.
`readonly` | `boolean \| undefined` | `false` | Makes the value read-only while keeping it focusable and selectable.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`placeholder` | `string \| undefined` |  | Text shown while the input is empty.
`name` | `string \| undefined` |  | Native `name` on the input; a plain `<form>` submits the formatted text.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; increment: UiPartValue; decrement: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `number \| null \| undefined` | `null` | Numeric value; `null` when the field is empty.

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Inline leading content, before the value and the `'split'` mode decrement button.
`end` | `any` | Inline trailing content, before the stepper buttons.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: number \| null]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element (the frame around the input).
`inputEl` | `HTMLInputElement \| null` | Native `<input>` element.
`increment` | `() => void` | Adds one `step`, clamped to `max`. No-op when disabled or read-only.
`decrement` | `() => void` | Subtracts one `step`, clamped to `min`. No-op when disabled or read-only. 

