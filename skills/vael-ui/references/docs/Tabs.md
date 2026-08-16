# Tabs

Switches between panels of content, with a sliding indicator under the active tab.

```ts
import { Tabs } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `T[]` |  | 
`orientation` | `"horizontal" \| "vertical" \| undefined` | "horizontal" | Vertical layout with ↑/↓ navigation.
`activation` | `"automatic" \| "manual" \| undefined` | "automatic" | `automatic` (default): arrow keys select. `manual`: arrow keys move focus only; Enter/Space selects.
`ui` | `Partial<{ list: UiPartValue; }> \| undefined` |  | 
`active` | `T` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ active: T; focused: T; select: (item: T) => void; items: T[]; }` | Render `role="tab"` elements and indicator. `focused` tracks roving tabindex (diverges from `active` in `manual` mode).

## Events

Name | Type | Description
--- | --- | ---
`change` | `[item: T]` | 
`update:active` | `[value: T]` | 

## Exposed

_None._

