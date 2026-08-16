# Tag

A compact label for categorizing or annotating an item inline.

```ts
import { Tag } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`variant` | `"primary" \| "danger" \| "muted" \| "success" \| "warning" \| "info" \| undefined` | "muted" | 
`size` | `"md" \| "sm" \| undefined` | "md" | 
`pill` | `boolean \| undefined` | false | Fully pill-rounded instead of the default small label corners.
`ui` | `Partial<{ root: UiPartValue; icon: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | 
`icon` | `any` | A small leading glyph (a dot, a checkmark) — sized to match the text, not a full icon box.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 

