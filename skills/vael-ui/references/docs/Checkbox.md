# Checkbox

A tri-state toggle for a single yes/no/indeterminate choice.

```ts
import { Checkbox } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`label` | `string \| undefined` |  | Label text; the default slot replaces it.
`value` | `string \| number \| undefined` |  | Value added to an array model when checked. Checked state reflects its membership.
`indeterminate` | `boolean \| undefined` | `false` | Shows the mixed (indeterminate) state, which takes precedence over checked.
`disabled` | `boolean \| undefined` | `false` | Disables the checkbox and blocks interaction.
`invalid` | `boolean \| undefined` | `false` | Shows the invalid state. A surrounding `Field` in error sets it too.
`size` | `"md" \| "sm" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | Native `name` for form submission.
`motionCss` | `boolean \| undefined` | `true` | `false` skips built-in transitions; animate the exposed `boxEl`/`checkEl` yourself.
`ui` | `Partial<{ root: UiPartValue; box: UiPartValue; label: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `boolean \| unknown[] \| undefined` | `false` | Checked state. Bind an array to add or remove this checkbox's `value` in it (checkbox group).

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Label content; replaces the `label` text.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: boolean \| unknown[]]` | Fires when `modelValue` changes (`v-model`).
`change` | `[checked: boolean]` | Fires when you toggle the checkbox, with its new checked state.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`inputEl` | `HTMLInputElement \| null` | Native checkbox input.
`boxEl` | `HTMLElement \| null` | Visual box element.
`checkEl` | `SVGElement \| null` | Check-mark SVG element. 

