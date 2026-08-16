# Dock

A macOS-style row of icons that magnify as the pointer approaches them.

```ts
import { Dock } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `DockItemData[]` |  | 
`orientation` | `DockOrientation \| undefined` | "horizontal" | 
`baseSize` | `number \| undefined` | 48 | Resting icon size, in px.
`maxSize` | `number \| undefined` | 76 | Icon size, in px, directly under the pointer.
`range` | `number \| undefined` |  | Falloff distance in px; defaults to 3.5x `baseSize`.
`disabled` | `boolean \| undefined` | false | 
`magnify` | `boolean \| undefined` | true | `false` disables magnification but keeps interaction enabled; distinct from `disabled`.
`tooltips` | `boolean \| undefined` | true | Renders each item's `v-tooltip` on hover.
`tooltipSide` | `Side \| undefined` | undefined | Which side each item's tooltip opens on. Default: `'top'` for horizontal, `'right'` for vertical.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; }> \| undefined` |  | 

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`select` | `[item: DockItemData, index: number]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`remeasure` | `() => void` | 

