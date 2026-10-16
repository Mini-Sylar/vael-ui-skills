# preventFocusZoom

iOS zooms the page when you focus a form field with text under 16px. vael-ui's fields follow your font size, so at 14px they would trigger it. Call `preventFocusZoom()` once and fields keep your size on iPhone without the zoom. It adds `maximum-scale=1` to the viewport on iOS only, where pinch-to-zoom keeps working. Android and desktop never focus-zoom, so it leaves them alone. If you'd rather have bigger fields on phones than change the viewport, set `--ui-touch-font-size: 16px` instead.

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
`changed` | `boolean` | `true` on iOS, where it set the viewport. `false` elsewhere, and during SSR.

