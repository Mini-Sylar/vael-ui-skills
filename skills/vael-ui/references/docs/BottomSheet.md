# BottomSheet

A panel that slides up from the bottom edge, common on touch layouts.

```ts
import { BottomSheet } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`title` | `string \| undefined` |  | Renders the default header with built-in close button.
`snapPoints` | `SheetSnapPoint[] \| undefined` |  | Ordered smallest to largest. Default: 60% / 92% of viewport height. Ignored when `fullScreen` is set and this is left unspecified.
`initialSnap` | `string \| undefined` |  | Which snap point to open at. Defaults to the first (smallest).
`dismissible` | `boolean \| undefined` | true | Whether dragging past the smallest snap point closes the sheet. Default true.
`width` | `"md" \| "sm" \| "lg" \| "full" \| undefined` | "full" | Panel width: `full` spans edge to edge, `sm`/`md`/`lg` cap and center it. Default: `full`.
`fullScreen` | `boolean \| undefined` | false | Single snap point covering entire viewport height. Shorthand for `snapPoints=[{ id: 'full', height: 1 }]`.
`closeOnEsc` | `boolean \| undefined` | true | Escape key closes the sheet.
`closeOnOverlay` | `boolean \| undefined` | true | Clicking the overlay closes the sheet.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` when complete. Fires for all close paths.
`ui` | `Partial<{ panel: UiPartValue; handleZone: UiPartValue; handle: UiPartValue; header: UiPartValue; title: UiPartValue; close: UiPartValue; content: UiPartValue; }> \| undefined` |  | Per-instance part-class/style overrides.
`open` | `boolean \| undefined` | false | Whether the sheet is open.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ activeSnap: string \| null; isDragging: boolean; isClosing: boolean; close: () => void; }` | 
`header` | `{ close: () => void; }` | Replaces the default title + close-button row entirely.

## Events

Name | Type | Description
--- | --- | ---
`update:open` | `[value: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | 
`activeSnap` | `string \| null` | 
`isDragging` | `boolean` | 
`isClosing` | `boolean` | 
`close` | `() => void` | 
`cancelClose` | `() => void` | 

