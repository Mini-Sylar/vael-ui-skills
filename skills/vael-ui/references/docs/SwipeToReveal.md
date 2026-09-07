# SwipeToReveal

Swipe an item to reveal actions underneath it, like a native mobile list row.

```ts
import { SwipeToReveal } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`disabled` | `boolean \| undefined` | false | 
`motionCss` | `boolean \| undefined` | true | `false` disables the built-in release/settle transition entirely (via `data-motion="off"`) — reach for this if you're driving the settle with your own spring/GSAP timeline instead. Has no effect on the drag itself, which is already transform-only with no transition.
`revealScale` | `number \| boolean \| undefined` | true | Grow the actions panel in from its own edge as the swipe reveals it, settling to full size. On by default; `false` opts out (e.g. when driving the panel with your own timeline), a number (0–1) sets the closed-state scale. The raw 0→1 reveal progress is always on the `progress` slot prop and the `--ui-swipe-reveal-progress` custom property for driving your own effects.
`ui` | `Partial<{ root: UiPartValue; content: UiPartValue; actions: UiPartValue; }> \| undefined` | undefined | 
`open` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ open: boolean; openSide: SwipeRevealSide \| null; reveal: (side?: SwipeRevealSide \| undefined) => void; close: () => void; }` | 
`leading-actions` | `{ open: boolean; close: () => void; progress: number; }` | Leading-edge (left, LTR) actions. `progress` is 0 → 1 as this edge is revealed.
`trailing-actions` | `{ open: boolean; close: () => void; progress: number; }` | Trailing-edge (right, LTR) actions. `progress` is 0 → 1 as this edge is revealed.

## Events

Name | Type | Description
--- | --- | ---
`update:open` | `[value: boolean]` | 
`change` | `[open: boolean, side: SwipeRevealSide \| null]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`contentEl` | `HTMLElement \| null` | 
`leadingActionsEl` | `HTMLElement \| null` | 
`trailingActionsEl` | `HTMLElement \| null` | 
`isDragging` | `boolean` | 
`openSide` | `SwipeRevealSide \| null` | 
`leadingProgress` | `number` | 0 → 1 reveal progress for each edge — live during a drag, settled after.
`trailingProgress` | `number` | 
`reveal` | `(side?: SwipeRevealSide \| undefined) => void` | 
`close` | `() => void` | 

