# SelectButton

A segmented control for choosing one (or several) values from a short list.

```ts
import { SelectButton } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Options to choose from.
`multiple` | `boolean \| undefined` | `false` | Turns on multi-select; the model becomes an array.
`allowEmpty` | `boolean \| undefined` | `true` | Single mode only: clicking the active option clears the model.
`disabled` | `boolean \| undefined` | `false` | Disables every option and blocks interaction.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | Native `name` for form submission. Auto-generated when omitted.
`ui` | `Partial<{ root: UiPartValue; option: UiPartValue; indicator: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | `null` | Selected value, an array of values with `multiple`, or `null` when nothing is selected.

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; checked: boolean; }` | Custom content for each option. Receives `item` and `checked`.

## Events

Name | Type | Description
--- | --- | ---
`change` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when you change the selection, with the new model value.
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`indicatorEl` | `HTMLElement \| null` | Sliding selection indicator (single mode only; `null` with `multiple`).

