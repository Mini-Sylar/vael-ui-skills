# RadioGroup

Coordinates a set of Radio buttons so only one can be selected at a time.

```ts
import { RadioGroup } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`name` | `string \| undefined` |  | Native `name` shared by every Radio's input. Auto-generated when omitted.
`disabled` | `boolean \| undefined` | `false` | Disables every Radio in the group.
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'vertical'` | Lays the radios out in a row or a column.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| number \| null \| undefined` | `null` | Value of the selected Radio, or `null` when none is selected.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | The group's `Radio` items.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string \| number \| null]` | Fires when `modelValue` changes (`v-model`).
`change` | `[value: string \| number \| null]` | Fires when you select a different Radio, with its value.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element. 

