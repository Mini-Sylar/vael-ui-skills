# useFloatingPosition

The positioning engine behind `Popover`, `Menu` and `Tooltip`, built on Floating UI. It computes `positionerStyle` (absolute `top` and `left`, plus `visibility`) for a reference and a floating element. It flips and shifts to stay on screen, and tracks changes with `autoUpdate` while `active` is `true`. Use it to build an anchored surface that no overlay component fits.

```ts
import { useFloatingPosition } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`referenceEl` | `Ref<HTMLElement \| null>` | The anchor element.
`floatingEl` | `Ref<HTMLElement \| null>` | The positioned surface.
`active` | `MaybeRefOrGetter<boolean>` | Turns positioning on. Scroll and resize tracking run only while it is `true`.
`side` | `MaybeRefOrGetter<Side>` | The side of the reference to place it on. It flips when there's no room. Default `'bottom'`.
`align` | `MaybeRefOrGetter<'start' \| 'center' \| 'end'>` | How it lines up along that side. Default `'center'`.
`sideOffset` | `MaybeRefOrGetter<number>` | The gap from the reference in px, along `side`. Default `8`.
`alignOffset` | `MaybeRefOrGetter<number>` | The shift along the alignment axis, in px. Default `0`.
`matchReferenceWidth` | `MaybeRefOrGetter<boolean>` | Writes the reference's width to `--ui-anchor-inline-size` in `positionerStyle`. Your positioner CSS can read that variable.

## Returns

Name | Type | Description
--- | --- | ---
`positionerStyle` | `Ref<Record<string, string>>` | The position to bind on the floating element: `:style="positionerStyle"`.
`placement` | `Ref<Placement>` | The resolved placement, after flipping.
`transformOrigin` | `Ref<string>` | Points at the reference's center, on the panel's facing edge, so a scale animation grows out of the reference.
`maxHeight` | `Ref<number \| null>` | The height left for the surface in px, or `null` while inactive.
`update` | `() => Promise<void>` | Measures again, for a change `autoUpdate` doesn't catch.

