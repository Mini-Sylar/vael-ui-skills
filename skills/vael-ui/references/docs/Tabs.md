# Tabs

Switches between panels of content, with a sliding indicator under the active tab.

```ts
import { Tabs } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `T[]` |  | Tab values, in order; drives keyboard navigation.
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'horizontal'` | Lays the tabs out in a row or a column; arrow keys follow the axis.
`activation` | `"automatic" \| "manual" \| undefined` | `'automatic'` | `'automatic'`: arrow keys select. `'manual'`: arrow keys move focus only, and `Enter` or `Space` selects.
`idBase` | `string \| undefined` |  | Prefix for the tab and panel ids. Set it when you render tab panels: each tab then gets `aria-controls="{idBase}-panel-{value}"`, so give each panel that id (or spread the exposed `panelProps(value)`). Values should be id-safe (no spaces).
`ui` | `Partial<{ list: UiPartValue; item: UiPartValue; indicator: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`active` | `T` |  | Active tab value.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ active: T; focused: T; select: (item: T) => void; items: T[]; itemProps: (item: T) => { role: "tab"; id: string; 'aria-controls': string \| undefined; class: string; style: StyleValue; 'data-tab-value': string; 'aria-selected': boolean; tabindex: 0 \| -1; onClick: () => void; }; indicatorProps: (variant?: "background" \| "underline" \| undefined) => { class: string; style: Record<string, string \| undefined>; }; tabId: (item: T) => string; panelId: (item: T) => string; }` | The tab buttons: bind `itemProps(item)` on each, and `indicatorProps()` on an optional sibling for the sliding highlight.

## Events

Name | Type | Description
--- | --- | ---
`change` | `[item: T]` | Fires when you select a different tab.
`update:active` | `[value: T]` | Fires when `active` changes (`v-model:active`).

## Exposed

Name | Type | Description
--- | --- | ---
`listEl` | `HTMLElement \| null` | Tab list (root) element.
`tabId` | `(item: T) => string` | The id a tab carries.
`panelId` | `(item: T) => string` | The id a tab's panel should carry.
`panelProps` | `(item: T) => { id: string; role: "tabpanel"; 'aria-labelledby': string; tabindex: 0; }` | `id`, `role="tabpanel"`, `aria-labelledby` and `tabindex` for a panel you render.

