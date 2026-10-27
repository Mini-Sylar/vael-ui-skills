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
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Control size.
`disabled` | `boolean \| undefined` | `false` | Disables the input and blocks interaction. A disabled parent Field also disables it.
`readonly` | `boolean \| undefined` | `false` | Makes the value read-only while keeping it focusable and selectable.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`placeholder` | `string \| undefined` |  | Text shown while the input is empty.
`name` | `string \| undefined` |  | Native `name` on the input; a plain `<form>` submits the formatted text.
`scrub` | `number \| boolean \| undefined` | `false` | Drag sideways on the field, while you're not typing in it, to change the value: one `step` per this many pixels, ten with Shift held. `true` is 4 px. A click without a drag still focuses the field for typing.
`animated` | `boolean \| undefined` | `false` | Rolls the digits to each new value from the steppers, the arrow keys or your code, like AnimatedNumber. Typing shows the plain input.
`motion` | `InputNumberMotion \| undefined` |  | How it moves: its appearance (`{ initial: { root: { opacity: 0, y: 4 } } }`) and its states (`{ states: { invalid: { root: { x: [0, -6, 6, 0] } } } }`). `false` turns its own animation off.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; increment: UiPartValue; decrement: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `number \| null \| undefined` | `null` | Numeric value; `null` when the field is empty.

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Inline leading content, before the value and the `'split'` mode decrement button.
`end` | `any` | Inline trailing content, before the stepper buttons.
`display` | `{ value: string; placeholder: string; focused: boolean; }` | Draws the text yourself over the field's own text, as Input's `#display`; it replaces the rolling digits of `animated`. `value` is the formatted text.

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
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `InputNumberMotion` (exported from `'vael-ui'`); `false` turns it off. InputNumber: its appearance and states.

- Parts: `root`, `input`, `increment`, `decrement`
- States: `invalid`, `disabled`, `focus`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

