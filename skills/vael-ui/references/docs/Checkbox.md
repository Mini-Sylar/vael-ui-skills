# Checkbox

A tri-state toggle for a single yes/no/indeterminate choice.

```ts
import { Checkbox } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`label` | `string \| undefined` |  | 
`value` | `string \| number \| undefined` |  | Only meaningful alongside an array model — checked reflects membership.
`indeterminate` | `boolean \| undefined` | false | Property, not attribute.
`disabled` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| undefined` | "md" | 
`name` | `string \| undefined` |  | 
`motionCss` | `boolean \| undefined` | true | `false` skips built-in transitions; use exposed `boxEl`/`checkEl` to drive animation instead.
`ui` | `Partial<{ root: UiPartValue; box: UiPartValue; label: UiPartValue; }> \| undefined` |  | 
`modelValue` | `boolean \| unknown[] \| undefined` | false | Checked state. Bind an array instead to toggle this checkbox's `value` prop in/out of it (checkbox-group pattern).

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Inline label content; overrides the `label` prop text entirely.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: boolean \| unknown[]]` | 
`change` | `[checked: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`inputEl` | `HTMLInputElement \| null` | 
`boxEl` | `HTMLElement \| null` | 
`checkEl` | `SVGElement \| null` | 

