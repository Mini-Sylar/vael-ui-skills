# Toaster

Renders the notifications created by toast(). Mount it once, anywhere in your app.

```ts
import { Toaster } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`position` | `ToasterPosition \| undefined` | "bottom-right" | 
`maxVisible` | `number \| undefined` | 4 | Max toasts shown at once; extras queue until visible slots free.
`gap` | `number \| undefined` | 10 | Spacing between stacked cards, px.
`teleportTo` | `string \| undefined` | "body" | 
`motionCss` | `boolean \| undefined` | true | `false` delegates enter/leave animations to `@card-enter`/`@card-leave` events.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ entry: ToastEntry; dismiss: () => void; depth: number; expanded: boolean; }` | Replaces a card's entire inner markup. The library still owns the <li> itself (position/stacking/swipe).

## Events

Name | Type | Description
--- | --- | ---
`card-enter` | `[el: Element, done: () => void]` | 
`card-leave` | `[el: Element, done: () => void]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`toasterEl` | `HTMLElement \| null` | 

