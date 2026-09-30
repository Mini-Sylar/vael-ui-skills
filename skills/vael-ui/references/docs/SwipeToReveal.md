# SwipeToReveal

Swipe an item to reveal actions underneath it, like a native mobile list row.

```ts
import { SwipeToReveal } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`disabled` | `boolean \| undefined` | `false` | Blocks swiping. `reveal()`, `close()` and `v-model:open` still work.
`motionCss` | `boolean \| undefined` | `true` | Plays the built-in release and settle transition. Set `false` to drive the settle yourself.
`revealScale` | `number \| boolean \| undefined` | `true` | Grows the actions panel in from its edge as it opens. A number from 0 to 1 sets the starting scale. `false` turns it off.
`ui` | `Partial<{ root: UiPartValue; content: UiPartValue; actions: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`open` | `boolean \| undefined` | `false` | Whether an action panel is open. Setting it `true` opens the trailing edge, or the leading one if it's the only edge.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ open: boolean; openSide: SwipeRevealSide \| null; reveal: (side?: SwipeRevealSide \| undefined) => void; close: () => void; }` | Row content that slides to reveal the actions.
`leading-actions` | `{ open: boolean; close: () => void; progress: number; }` | Leading-edge actions (left in LTR). `progress` goes from 0 to 1 as this edge opens.
`trailing-actions` | `{ open: boolean; close: () => void; progress: number; }` | Trailing-edge actions (right in LTR). `progress` goes from 0 to 1 as this edge opens.

## Events

Name | Type | Description
--- | --- | ---
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`change` | `[open: boolean, side: SwipeRevealSide \| null]` | Fires once per settled interaction, with `open` and the open edge (or `null`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`contentEl` | `HTMLElement \| null` | Sliding content element.
`leadingActionsEl` | `HTMLElement \| null` | Leading actions panel (null without `#leading-actions`).
`trailingActionsEl` | `HTMLElement \| null` | Trailing actions panel (null without `#trailing-actions`).
`isDragging` | `boolean` | Whether you're swiping. Taps don't count.
`openSide` | `SwipeRevealSide \| null` | Which edge is open, or `null` when closed.
`leadingProgress` | `number` | Reveal progress of the leading edge, from 0 to 1. Live during a drag, settled after.
`trailingProgress` | `number` | Reveal progress of the trailing edge, from 0 to 1. Live during a drag, settled after.
`reveal` | `(side?: SwipeRevealSide \| undefined) => void` | Opens an edge. Without `side`, it opens the trailing edge, or the leading one if it's the only edge.
`close` | `() => void` | Closes the open edge. 

