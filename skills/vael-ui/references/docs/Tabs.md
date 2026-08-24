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
`default` | `{ active: T; focused: T; select: (item: T) => void; items: T[]; itemProps: (item: T) => { role: "tab"; class: string; style: UiPartStyle \| undefined; 'data-tab-value': string; 'aria-selected': boolean; tabindex: 0 \| -1; onClick: () => void; }; indicatorProps: (variant?: "background" \| "underline" \| undefined) => { class: string; style: Record<string, string \| undefined>; }; }` | Render `role="tab"` elements and, optionally, a sliding indicator. `focused` tracks roving tabindex (diverges from `active` in `manual` mode). `itemProps(item)` returns the full a11y/behavior wiring for one tab — spread it with `v-bind` onto whatever element you render, a plain `<button>` or `<Button>`. `indicatorProps(variant)` does the same for the optional sliding highlight (`'background'` default, or `'underline'`) — render one element with it bound as a sibling of the tab buttons.

## Events

Name | Type | Description
--- | --- | ---
`change` | `[item: T]` | 
`update:active` | `[value: T]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`listEl` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.

