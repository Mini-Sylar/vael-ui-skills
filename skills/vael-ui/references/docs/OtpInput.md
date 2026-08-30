# OtpInput

A row of single-character boxes for one-time codes, with paste and auto-advance.

```ts
import { OtpInput } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`length` | `number \| undefined` | 6 | 
`type` | `"numeric" \| "alphanumeric" \| undefined` | "numeric" | Restricts input to digits only or alphanumeric.
`mask` | `boolean \| undefined` | false | Shows a bullet instead of the entered character in every filled cell.
`disabled` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`name` | `string \| undefined` |  | 
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; cell: UiPartValue; }> \| undefined` |  | 
`modelValue` | `string \| undefined` | "" | 

## Slots

Name | Type | Description
--- | --- | ---
`cell` | `{ char: string \| null; index: number; active: boolean; filled: boolean; }` | Custom content for one cell.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string]` | 
`complete` | `[code: string]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`inputEl` | `HTMLInputElement \| null` | 
`cellEls` | `HTMLElement[] \| null` | 

