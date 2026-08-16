# Input

A single-line text field with sizes, states, and slots for icons or hints.

```ts
import { Input } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`type` | `string \| undefined` | "text" | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`disabled` | `boolean \| undefined` | false | 
`readonly` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | Standalone override; ORed with the nearest Field's `error` state.
`placeholder` | `string \| undefined` |  | 
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; start: UiPartValue; end: UiPartValue; }> \| undefined` |  | 
`modelValue` | `string \| undefined` | "" | 

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Inline leading content (icon, kbd hint, a copy Button).
`end` | `any` | Inline trailing content.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`inputEl` | `HTMLInputElement \| null` | 

