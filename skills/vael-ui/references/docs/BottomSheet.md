# BottomSheet

A panel that slides up from the bottom edge, common on touch layouts.

```ts
import { BottomSheet } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`title` | `string \| undefined` |  | Renders the default header with a built-in close button.
`snapPoints` | `SheetSnapPoint[] \| undefined` |  | Snap points, ordered smallest to largest. Unset, the sheet snaps at 60% and 92% of viewport height, or fills it when `fullScreen` is set.
`initialSnap` | `string \| undefined` |  | `id` of the snap point to open at. Unset, the sheet opens at the first (smallest).
`dismissible` | `boolean \| undefined` | `true` | Dragging past the smallest snap point closes the sheet.
`width` | `"md" \| "sm" \| "lg" \| "full" \| undefined` | `'full'` | Panel width: `'full'` spans edge to edge; `'sm'`, `'md'` and `'lg'` cap and center it.
`fullScreen` | `boolean \| undefined` | `false` | Uses a single snap point that covers the full viewport height. Shorthand for `[{ id: 'full', height: 1 }]` snap points.
`closeOnEsc` | `boolean \| undefined` | `true` | Closes the sheet when you press Escape.
`closeOnOverlay` | `boolean \| undefined` | `true` | Closes the sheet when you click the overlay.
`modal` | `boolean \| undefined` | `true` | `false` disables the overlay, scroll lock and focus trap, so the page behind stays interactive.
`closeOnHistoryBack` | `boolean \| undefined` | `false` | Pushes a history entry on open, so the mobile back gesture or button closes the sheet. The page doesn't navigate away.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing. Runs for every close path.
`ui` | `Partial<{ overlay: UiPartValue; panel: UiPartValue; handleZone: UiPartValue; handle: UiPartValue; header: UiPartValue; title: UiPartValue; close: UiPartValue; content: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether the sheet is open.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ activeSnap: string \| null; isDragging: boolean; isClosing: boolean; close: () => void; }` | Sheet content.
`header` | `{ close: () => void; }` | Replaces the default title and close-button row.

## Events

Name | Type | Description
--- | --- | ---
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).

## Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`activeSnap` | `string \| null` | `id` of the snap point the sheet rests at.
`isDragging` | `boolean` | Whether you're dragging the sheet.
`isClosing` | `boolean` | Whether a close is in progress (the exit animation or `beforeClose` hasn't finished).
`close` | `() => void` | Closes the sheet with its slide-out animation.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the sheet open. 

