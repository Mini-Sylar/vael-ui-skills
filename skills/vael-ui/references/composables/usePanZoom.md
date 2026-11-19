# usePanZoom

Pans, pinches and zooms content inside a box: an image, a map, a diagram. It moves the content with its own `translate` and `scale`, from its top left corner. One finger or the mouse pans, two fingers pinch, and a double tap zooms in and back out. Hold ctrl and scroll to zoom around the cursor; in Safari, a trackpad pinch does the same. Content stays inside the box, gives a little at the edges, and keeps coasting after a flick. It sets `touch-action: none` on the box, so the page does not scroll there on touch.

```ts
import { usePanZoom } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`options.el` | `MaybeRefOrGetter<HTMLElement \| null>` | The box you pan and zoom in. It receives the pointer, the wheel and the focus.
`options.content` | `MaybeRefOrGetter<HTMLElement \| null>` | The element that moves. Default: the first child of `el`.
`options.minScale` | `MaybeRefOrGetter<number>` | The smallest scale. Default `1`, the content at its own size.
`options.maxScale` | `MaybeRefOrGetter<number>` | The largest scale. Default `4`.
`options.doubleTapScale` | `MaybeRefOrGetter<number>` | The scale a double tap zooms to. Default `2`.
`options.disabled` | `MaybeRefOrGetter<boolean>` | Ignores the pointer, the wheel and the keys.
`options.spring` | `MaybeRefOrGetter<Spring \| SpringName>` | The spring it settles on: a name or `{ damping, response }`. Default: the ConfigProvider's `spring`, else the default every component uses.

## Returns

Name | Type | Description
--- | --- | ---
`x, y` | `Ref<number>` | Where the top left of the content is, in px from where it sits at rest.
`scale` | `Ref<number>` | How big the content is. `1` is its own size.
`isDragging` | `Ref<boolean>` | Whether a pointer is panning or pinching.
`isZoomed` | `ComputedRef<boolean>` | Whether it is zoomed in past `minScale`.
`zoomTo(scale, { at?, animate? })` | `(scale: number, options?) => void` | Zooms to `scale` around `at`, a point in px inside `el`. Default: its centre.
`zoomBy(factor, { at?, animate? })` | `(factor: number, options?) => void` | Zooms by a factor from where it is: `1.5` in, `1 / 1.5` out.
`panBy({ x, y }, { animate? })` | `(by: { x, y }, options?) => void` | Moves by `by` px, kept inside the content.
`reset({ animate? })` | `(options?) => void` | Back to `minScale`, centred.
`onKeydown` | `(event: KeyboardEvent) => void` | `+`, `-`, `0` and the arrow keys. Bind it to `@keydown` on `el`.

