# SwipeToReveal

Swipe an item to reveal actions underneath it, like a native mobile list row.

```ts
import { SwipeToReveal } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`side` | `SwipeRevealSide \| undefined` | "trailing" | 
`disabled` | `boolean \| undefined` | false | 
`motionCss` | `boolean \| undefined` | true | `false` disables the built-in release/settle transition entirely (via `data-motion="off"`) — reach for this if you're driving the settle with your own spring/GSAP timeline instead. Has no effect on the drag itself, which is already transform-only with no transition.
`ui` | `Partial<{ root: UiPartValue; content: UiPartValue; actions: UiPartValue; }> \| undefined` | undefined | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ open: boolean; reveal: () => void; close: () => void; }` | 
`actions` | `{ open: boolean; close: () => void; }` | 

## Events

Name | Type | Description
--- | --- | ---
`update:open` | `[value: boolean]` | 
`change` | `[open: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`contentEl` | `HTMLElement \| null` | 
`actionsEl` | `HTMLElement \| null` | 
`isDragging` | `boolean` | 
`reveal` | `() => void` | 
`close` | `() => void` | 

