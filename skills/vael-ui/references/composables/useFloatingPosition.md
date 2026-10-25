# useFloatingPosition

The positioning engine behind `Popover`, `Menu` and `Tooltip`, built on Floating UI. It computes `positionerStyle` (absolute `top` and `left`, plus `visibility`) for a reference and floating element pair. It flips and shifts to avoid collisions, and tracks changes with `autoUpdate` while `active` is `true`. Use it directly to build a custom anchored surface that no existing overlay component fits.

```ts
import { useFloatingPosition } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`referenceEl` | `Ref<HTMLElement \| null>` | The anchor element.
`floatingEl` | `Ref<HTMLElement \| null>` | The positioned surface.
`active` | `MaybeRefOrGetter<boolean>` | Positioning and `autoUpdate` scroll and resize tracking run only while this is `true`.
`side` | `MaybeRefOrGetter<Side>` | Default `'bottom'`.
`align` | `MaybeRefOrGetter<'start' \| 'center' \| 'end'>` | Default `'center'`.
`sideOffset` | `MaybeRefOrGetter<number>` | Gap from the reference, along `side`. Default `8`.
`alignOffset` | `MaybeRefOrGetter<number>` | Shift along the alignment axis. Default `0`.
`matchReferenceWidth` | `MaybeRefOrGetter<boolean>` | Writes the reference's width to `--ui-anchor-inline-size` in `positionerStyle`. Your positioner CSS can read that variable.

## Returns

Name | Type | Description
--- | --- | ---
`positionerStyle` | `Ref<Record<string, string>>` | Bind it directly: `:style="positionerStyle"`.
`placement` | `Ref<Placement>` | The resolved placement, after flipping.
`transformOrigin` | `Ref<string>` | Matches the resolved placement, so scale and fade animations start at the anchor.
`maxHeight` | `Ref<number \| null>` | The available height for the surface, or `null` while inactive.
`update` | `() => Promise<void>` | Forces a recompute outside the normal `autoUpdate` triggers.
