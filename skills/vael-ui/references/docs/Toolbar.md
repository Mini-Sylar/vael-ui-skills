# Toolbar

A horizontal strip for grouping related actions, with roving keyboard focus.

```ts
import { Toolbar } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'horizontal'` | Lays the controls out in a row or a column; arrow keys follow the axis.
`overflowLabel` | `string \| undefined` | `'More'` | Accessible label for the overflow (`…`) menu button.
`ui` | `Partial<{ root: UiPartValue; group: UiPartValue; overflowTrigger: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Controls at the start of the toolbar. Mark a child `data-toolbar-overflow` to let it collapse into the `…` menu.
`default` | `any` | Controls in the start group, after `#start`.
`center` | `any` | Controls in the center group.
`end` | `any` | Controls at the end of the toolbar, before the `…` overflow menu.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element. 

