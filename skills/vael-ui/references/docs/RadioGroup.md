# RadioGroup

Coordinates a set of Radio buttons so only one can be selected at a time.

```ts
import { RadioGroup } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`name` | `string \| undefined` |  | 
`disabled` | `boolean \| undefined` | false | Disables all Radio children.
`orientation` | `"horizontal" \| "vertical" \| undefined` | "vertical" | 
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | 
`modelValue` | `string \| number \| null \| undefined` | null | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{}` | 

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string \| number \| null]` | 
`change` | `[value: string \| number \| null]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 

