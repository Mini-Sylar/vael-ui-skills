# SelectButton

A segmented control for choosing one (or several) values from a short list.

```ts
import { SelectButton } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | 
`multiple` | `boolean \| undefined` | false | 
`allowEmpty` | `boolean \| undefined` | true | Single mode only: clicking the active option clears the model.
`disabled` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`name` | `string \| undefined` |  | 
`ui` | `Partial<{ root: UiPartValue; option: UiPartValue; indicator: UiPartValue; }> \| undefined` |  | 
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | null | 

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; checked: boolean; }` | 

## Events

Name | Type | Description
--- | --- | ---
`change` | `[value: string \| number \| (string \| number)[] \| null]` | 
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`indicatorEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.

