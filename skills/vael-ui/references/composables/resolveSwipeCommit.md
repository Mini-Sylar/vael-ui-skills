# resolveSwipeCommit

The headless engine behind `<SwipeToReveal>`.

The pure open or close decision `useSwipeReveal` makes on release. A flick faster than 0.11 px/ms opens when it moved toward open and closes when it moved away. A slower drag opens when more than half the panel shows. Use it to give a gesture of your own the same feel.

```ts
import { resolveSwipeCommit } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`velocity` | `number` | Drag speed in px/ms, never negative. `useSwipeReveal` passes the distance moved divided by the time since the press.
`towardOpen` | `boolean` | Whether the drag moved toward the open side overall.
`openFraction` | `number` | How far open the panel is at release, as a fraction of the panel's width. It can go below 0 or above 1 while rubber-banding.

## Returns

Name | Type | Description
--- | --- | ---
`return` | `boolean` | `true` to settle open, `false` to settle closed.
