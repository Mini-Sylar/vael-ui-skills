# Tag

A compact label for categorizing or annotating an item inline.

```ts
import { Tag } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`variant` | `"primary" \| "danger" \| "muted" \| "success" \| "warning" \| "info" \| undefined` | `'muted'` | Color variant.
`size` | `"md" \| "sm" \| undefined` | `'md'` | Text size and padding.
`pill` | `boolean \| undefined` | `false` | Fully pill-rounded instead of the default small label corners.
`ui` | `Partial<{ root: UiPartValue; icon: UiPartValue; label: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Tag label.
`icon` | `any` | Small leading glyph, such as a dot or checkmark, sized to match the text rather than a full icon box.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element. 

