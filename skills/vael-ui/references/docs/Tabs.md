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
`ui` | `Partial<{ list: UiPartValue; item: UiPartValue; indicator: UiPartValue; }> \| undefined` |  | 
`active` | `T` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ active: T; focused: T; select: (item: T) => void; items: T[]; itemProps: (item: T) => { role: "tab"; class: string; style: StyleValue; 'data-tab-value': string; 'aria-selected': boolean; tabindex: 0 \| -1; onClick: () => void; }; indicatorProps: (variant?: "background" \| "underline" \| undefined) => { class: string; style: Record<string, string \| undefined>; }; }` | Render `role="tab"` elements and optionally a sliding indicator. `itemProps(item)` returns a11y/behavior wiring for one tab — spread via v-bind. `indicatorProps(variant)` does the same for the optional sliding highlight (`'background'` default, or `'underline'`) — bind it on a sibling element of the tab buttons. `focused` tracks roving tabindex (diverges from `active` in `manual` mode).

## Events

Name | Type | Description
--- | --- | ---
`change` | `[item: T]` | 
`update:active` | `[value: T]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`listEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.

