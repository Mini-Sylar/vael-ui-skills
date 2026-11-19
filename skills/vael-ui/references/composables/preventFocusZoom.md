# preventFocusZoom

Stops iOS from zooming the page when you focus a field. iOS zooms on any field with text under 16 px, and vael-ui's fields follow your font size, so at 14 px they trigger it. Call `preventFocusZoom()` once and fields keep your size on iPhone without the zoom. It adds `maximum-scale=1` to the viewport on iOS only, where pinch-to-zoom keeps working. Android and desktop never zoom on focus, so it leaves them alone. To get bigger fields on phones instead of changing the viewport, set `--ui-touch-font-size: 16px`.

```ts
import { preventFocusZoom } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```ts
// main.ts
import { preventFocusZoom } from 'vael-ui'

preventFocusZoom()
```

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`changed` | `boolean` | Whether it changed the viewport: `true` on iOS, `false` elsewhere and during server rendering.

