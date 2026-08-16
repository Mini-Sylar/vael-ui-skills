# Switch

An on/off toggle styled like a physical switch, for immediate-effect settings.

```ts
import { Switch } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`label` | `string \| undefined` |  | 
`disabled` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| undefined` | "md" | 
`name` | `string \| undefined` |  | 
`motionCss` | `boolean \| undefined` | true | 
`ui` | `Partial<{ root: UiPartValue; track: UiPartValue; thumb: UiPartValue; label: UiPartValue; }> \| undefined` |  | 
`modelValue` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | 

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`inputEl` | `HTMLInputElement \| null` | 
`trackEl` | `HTMLElement \| null` | 
`thumbEl` | `HTMLElement \| null` | 

