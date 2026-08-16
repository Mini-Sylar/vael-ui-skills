# useFloatingPosition

The Floating UI-backed positioning engine behind `Popover`, `Menu`, and `Tooltip`. Computes `positionerStyle` (absolute inset + `visibility`) against a reference/floating element pair, with flip/shift collision handling and live `autoUpdate` tracking while `active` is true. Reach for this directly when building a custom anchored surface none of the existing overlay components fit.

## Parameters

Name | Type | Description
--- | --- | ---
`referenceEl` | `Ref<HTMLElement \| null>` | The anchor.
`floatingEl` | `Ref<HTMLElement \| null>` | The positioned surface.
`active` | `MaybeRefOrGetter<boolean>` | Positioning (and autoUpdate scroll/resize tracking) only runs while true.
`side` | `MaybeRefOrGetter<Side>` | Default 'bottom'.
`align` | `MaybeRefOrGetter<'start' \| 'center' \| 'end'>` | Default 'center'.
`sideOffset` | `MaybeRefOrGetter<number>` | Gap along side. Default 8.
`alignOffset` | `MaybeRefOrGetter<number>` | Shift along the align axis.
`matchReferenceWidth` | `MaybeRefOrGetter<boolean>` | Writes the reference’s width into --ui-anchor-inline-size for the positioner to opt into.

## Returns

Name | Type | Description
--- | --- | ---
`positionerStyle` | `Ref<Record<string, string>>` | Bind directly: :style="positionerStyle".
`placement` | `Ref<Placement>` | The resolved placement, post-flip.
`transformOrigin` | `Ref<string>` | For scale/fade animations anchored correctly.
`maxHeight` | `Ref<number \| null>` | Available space in the resolved direction, or null.
`update` | `() => Promise<void>` | Force a recompute outside the normal auto-update triggers.

