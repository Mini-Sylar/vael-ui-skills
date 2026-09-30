# Toaster

Renders the notifications created by toast(). Mount it once, anywhere in your app.

```ts
import { Toaster } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`position` | `ToasterPosition \| undefined` | `'bottom-right'` | Screen corner or edge the toasts stack from.
`maxVisible` | `number \| undefined` | `4` | Max toasts shown at once; extras queue until visible slots free.
`gap` | `number \| undefined` | `10` | Spacing between stacked cards, in pixels.
`teleportTo` | `string \| undefined` | `'body'` | CSS selector to teleport the toast stack to.
`motionCss` | `boolean \| undefined` | `true` | `false` delegates enter/leave animations to `@card-enter`/`@card-leave` events.
`ui` | `Partial<{ root: UiPartValue; toast: UiPartValue; icon: UiPartValue; content: UiPartValue; title: UiPartValue; description: UiPartValue; action: UiPartValue; close: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ entry: ToastEntry; dismiss: () => void; depth: number; expanded: boolean; }` | Replaces a toast's inner markup; the toaster still handles its positioning, stacking and swipe.

## Events

Name | Type | Description
--- | --- | ---
`card-enter` | `[el: Element, done: () => void]` | Fires when a toast enters, only while `motionCss` is `false`. Run your own animation, then call `done()`.
`card-leave` | `[el: Element, done: () => void]` | Same as `@card-enter`, for a toast leaving.

## Exposed

Name | Type | Description
--- | --- | ---
`toasterEl` | `HTMLElement \| null` | Toast list (root) element. 

