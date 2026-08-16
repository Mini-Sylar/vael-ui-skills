# ScrollArea

```ts
import { ScrollArea } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`orientation` | `"horizontal" \| "vertical" \| "both" \| undefined` | "vertical" | 
`scrollFade` | `boolean \| undefined` | true | Masks the scrolling edge(s) as content scrolls under them.
`autoHide` | `boolean \| undefined` | false | Scrollbar thumb is transparent until you hover/scroll the viewport (Chromium/WebKit only — `::-webkit-scrollbar-thumb` has no hover-reveal equivalent for Firefox's `scrollbar-color`, which always shows the thin thumb).
`ui` | `Partial<{ root: UiPartValue; viewport: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | 

## Events

Name | Type | Description
--- | --- | ---
`scroll` | `[event: Event]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`viewportEl` | `HTMLElement \| null` | 
`scrollTop` | `number` | 
`scrollLeft` | `number` | 
`atTop` | `boolean` | 
`atBottom` | `boolean` | 
`atStart` | `boolean` | 
`atEnd` | `boolean` | 
`scrollTo` | `(options: ScrollToOptions) => void` | 
`scrollToTop` | `(options?: Omit<ScrollToOptions, "top"> \| undefined) => void` | 
`scrollToBottom` | `(options?: Omit<ScrollToOptions, "top"> \| undefined) => void` | 
`scrollToStart` | `(options?: Omit<ScrollToOptions, "left"> \| undefined) => void` | 
`scrollToEnd` | `(options?: Omit<ScrollToOptions, "left"> \| undefined) => void` | 

