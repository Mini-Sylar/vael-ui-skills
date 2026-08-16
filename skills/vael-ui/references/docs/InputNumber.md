# InputNumber

A numeric field with increment/decrement controls and format-aware parsing.

```ts
import { InputNumber } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` |  | 
`max` | `number \| undefined` |  | 
`step` | `number \| undefined` | 1 | 
`locale` | `string \| undefined` |  | 
`mode` | `"decimal" \| "currency" \| "percent" \| undefined` | "decimal" | 
`currency` | `string \| undefined` |  | ISO 4217 currency code — required when `mode="currency"`.
`minFractionDigits` | `number \| undefined` |  | 
`maxFractionDigits` | `number \| undefined` |  | 
`useGrouping` | `boolean \| undefined` | true | 
`prefix` | `string \| undefined` |  | Literal affix rendered outside Intl, e.g. a unit label.
`suffix` | `string \| undefined` |  | 
`controls` | `boolean \| undefined` | true | 
`stepperPosition` | `"split" \| "end" \| undefined` | "end" | `'end'` (default): stacked +/- column after the value. `'split'`: one full-height button on each side.
`allowEmpty` | `boolean \| undefined` | true | `false` coerces a blur-empty field to `min ?? 0` instead of `null`.
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`disabled` | `boolean \| undefined` | false | 
`readonly` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`placeholder` | `string \| undefined` |  | 
`name` | `string \| undefined` |  | 
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; increment: UiPartValue; decrement: UiPartValue; }> \| undefined` |  | 
`modelValue` | `number \| null \| undefined` | null | 

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | 
`end` | `any` | 

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: number \| null]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`inputEl` | `HTMLInputElement \| null` | 
`increment` | `() => void` | 
`decrement` | `() => void` | 

