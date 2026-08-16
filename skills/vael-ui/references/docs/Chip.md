# Chip

A removable, compact token for selected filters, tags, or multi-select values.

```ts
import { Chip } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`label` | `string \| undefined` |  | 
`removable` | `boolean \| undefined` | false | 
`disabled` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| undefined` | "md" | 
`ui` | `Partial<{ root: UiPartValue; label: UiPartValue; remove: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Overrides the label content; the library still owns the remove button and its accessible name (from the `label` prop) — pass `label` even when using this slot.

## Events

Name | Type | Description
--- | --- | ---
`remove` | `[]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 

