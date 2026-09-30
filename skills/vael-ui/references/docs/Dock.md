# Dock

A macOS-style row of icons that magnify as the pointer approaches them.

```ts
import { Dock } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `DockItemData[]` |  | Items to show, one button each.
`orientation` | `DockOrientation \| undefined` | `'horizontal'` | Lays items out in a row or a column; arrow keys follow the same axis.
`baseSize` | `number \| undefined` | `48` | Resting icon size, in pixels.
`maxSize` | `number \| undefined` | `76` | Icon size, in pixels, directly under the pointer.
`range` | `number \| undefined` |  | Falloff distance in pixels. Unset, it's 3.5 times `baseSize`.
`disabled` | `boolean \| undefined` | `false` | Dims the dock and blocks pointer interaction and magnification.
`magnify` | `boolean \| undefined` | `true` | `false` turns off magnification but keeps items interactive, unlike `disabled`.
`grow` | `boolean \| undefined` | `false` | Magnifies by resizing items, so the dock grows with them. Off, items scale and spread with transforms and the dock keeps its size.
`tooltips` | `boolean \| undefined` | `true` | Renders each item's `v-tooltip` on hover.
`tooltipSide` | `Side \| undefined` |  | Which side each item's tooltip opens on. Unset, it's `'top'` when horizontal and `'right'` when vertical.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`select` | `[item: DockItemData, index: number]` | Fires when you click an item, or press Enter or Space on a focused item.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`remeasure` | `() => void` | Re-measures item positions for magnification, after a layout change the dock can't detect. 

