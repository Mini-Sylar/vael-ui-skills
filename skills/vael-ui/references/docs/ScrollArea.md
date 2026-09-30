# ScrollArea

```ts
import { ScrollArea } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`orientation` | `"horizontal" \| "vertical" \| "both" \| undefined` | `'vertical'` | Which axis (or axes) the viewport scrolls along.
`scrollFade` | `boolean \| undefined` | `true` | Masks the scrolling edge(s) as content scrolls under them.
`autoHide` | `boolean \| undefined` | `false` | Hides the scrollbar thumb until you hover, focus or scroll the viewport.
`ui` | `Partial<{ root: UiPartValue; viewport: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Scrollable content.

## Events

Name | Type | Description
--- | --- | ---
`scroll` | `[event: Event]` | Fires when the viewport scrolls.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`viewportEl` | `HTMLElement \| null` | Scrolling viewport element.
`scrollTop` | `number` | Vertical scroll offset in pixels. Writable: assigning it scrolls the viewport.
`scrollLeft` | `number` | Horizontal scroll offset in pixels. Writable: assigning it scrolls the viewport.
`atTop` | `boolean` | Whether the viewport is scrolled to the top.
`atBottom` | `boolean` | Whether the viewport is scrolled to the bottom.
`atStart` | `boolean` | Whether the viewport is scrolled to the left edge.
`atEnd` | `boolean` | Whether the viewport is scrolled to the right edge.
`isScrolling` | `boolean` | `true` while a scroll is in progress, including native momentum or rubber-band settling.
`directions` | `{ left: boolean; right: boolean; top: boolean; bottom: boolean; }` | Direction of the current scroll, as `top`, `bottom`, `left` and `right` flags.
`scrollTo` | `(options: ScrollToOptions) => void` | Scrolls the viewport with native `ScrollToOptions`.
`scrollToTop` | `(options?: Omit<ScrollToOptions, "top"> \| undefined) => void` | Smooth-scrolls to the top.
`scrollToBottom` | `(options?: Omit<ScrollToOptions, "top"> \| undefined) => void` | Smooth-scrolls to the bottom.
`scrollToStart` | `(options?: Omit<ScrollToOptions, "left"> \| undefined) => void` | Smooth-scrolls to the left edge.
`scrollToEnd` | `(options?: Omit<ScrollToOptions, "left"> \| undefined) => void` | Smooth-scrolls to the right edge. 

