# useSheetDrag

The headless engine behind `<BottomSheet>`.

The drag and snap engine behind `<BottomSheet>`, modeled on Vaul. The panel stays as tall as its area and only moves with `translateY`, so a drag never triggers a resize. A release settles on a snap point, steps one point for a quick short flick, or asks you to dismiss. It decides from the speed over the last 80 ms, so a slow drag that ends in a flick reads as a flick. A drag on the content only moves the sheet when the content is scrolled to the top and the drag goes down. `<BottomSheet>` adds the overlay, focus trap, enter and exit, the close flow and nested sheet detection.

```ts
import { useSheetDrag } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`active` | `Ref<boolean>` | First argument. Turning it `true` places the panel off screen, waits two frames, then slides it to `initialSnap`. Turning it `false` snaps the panel off screen at once.
`panelEl` | `Ref<HTMLElement \| null>` | The element that moves. The composable owns its `transform`, `transition` and `block-size` inline styles. It also sets `--sheet-visible-height` to the height on screen, for capping a scroll body inside.
`handleEl` | `Ref<HTMLElement \| null>` | A press here always starts a drag.
`contentEl` | `Ref<HTMLElement \| null>` | The scrolling body. A press here becomes a sheet drag only when the body is at scroll top and the move is downward and mostly vertical. A mostly horizontal move is left to nested gestures, such as a swipe-to-reveal row.
`snapPoints` | `MaybeRefOrGetter<SheetSnapPoint[]>` | Required. `{ id, height }` entries ordered from smallest to largest. `height` is a fraction of the area: `0.6` shows 60% of it.
`initialSnap` | `MaybeRefOrGetter<string \| undefined>` | The `id` to open at. Default: the first snap point.
`bounds` | `MaybeRefOrGetter<HTMLElement \| null \| undefined>` | An element the sheet fills instead of the viewport. Its resizes refit the panel, the same way viewport and on-screen keyboard changes do.
`dismissible` | `MaybeRefOrGetter<boolean>` | Default `true`. `false` returns a drag past the smallest snap point to it instead of dismissing.
`onDismiss` | `(velocity: number) => void` | Required. Fires when a release decides to dismiss, with the panel left where the drag ended. Run your close flow here. `velocity` is the release speed in px/ms, positive toward closed.

## Returns

Name | Type | Description
--- | --- | ---
`activeSnap` | `Ref<string \| null>` | The `id` of the snap point the sheet rests at. Read it only: writing it does not move the panel.
`isDragging` | `Ref<boolean>` | `true` while the sheet follows a pointer.
`reset` | `() => Promise<void>` | Slides the panel in to `initialSnap` again. `active` turning `true` already calls it.
`setReceded` | `(receded: boolean) => void` | Scales the panel down and shifts it up 16 px while another sheet stacks on top, from its current snap point. `<BottomSheet>` calls it on the parent sheet when a nested one opens.
