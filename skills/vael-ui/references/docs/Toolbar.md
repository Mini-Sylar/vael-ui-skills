# Toolbar

A horizontal strip for grouping related actions, with roving keyboard focus.

```ts
import { Toolbar } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`orientation` | `"horizontal" \| "vertical" \| undefined` | "horizontal" | 
`overflowLabel` | `string \| undefined` | "More" | 
`ui` | `Partial<{ root: UiPartValue; group: UiPartValue; overflowTrigger: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`start` | `{}` | 
`default` | `{}` | 
`center` | `{}` | 
`end` | `{}` | 

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 

