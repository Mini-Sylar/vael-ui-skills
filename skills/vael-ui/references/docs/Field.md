# Field

Wraps a form control with a label, help text, and error message, wired together for you.

```ts
import { Field } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`label` | `string \| undefined` |  | 
`description` | `string \| undefined` |  | 
`error` | `string \| undefined` |  | Error message; renders with `role="alert"`.
`required` | `boolean \| undefined` |  | 
`disabled` | `boolean \| undefined` |  | 
`labelPlacement` | `"top" \| "float" \| "inset" \| undefined` | "top" | 
`ui` | `Partial<{ root: UiPartValue; label: UiPartValue; control: UiPartValue; description: UiPartValue; error: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | 
`label` | `any` | Replaces label text without affecting label element or for wiring.
`description` | `any` | 
`error` | `{ error: string; }` | 

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 

