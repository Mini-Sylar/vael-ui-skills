# dockFalloff, dockItemSize, dockItemSizes

The headless engine behind `<Dock>`.

The pure, DOM-free size math `useDock` runs on every pointer move. Use them to drive your own magnification, or to preview the curve in a test or a chart. Distances are in px along the dock.

```ts
import { dockFalloff, dockItemSize, dockItemSizes } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```ts
import { dockItemSizes } from 'vael-ui'

// Four items 56 px apart, pointer over the second one.
dockItemSizes(56, [0, 56, 112, 168], { baseSize: 48, maxSize: 76, range: 168 })
```

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`dockFalloff(distance, range)` | `number` | A raised cosine from `1` at distance 0 to `0` at `range` and beyond. The sign of `distance` is ignored. Returns `0` when `range` is 0 or less.
`dockItemSize(distance, options)` | `number` | The item size in px at `distance` from the pointer: `baseSize` plus `maxSize - baseSize` times the falloff. `options` is `{ baseSize, maxSize, range }`.
`dockItemSizes(pointerPosition, centers, options)` | `number[]` | Every item's size at once, from the pointer position and each item's resting centre along the same axis.
