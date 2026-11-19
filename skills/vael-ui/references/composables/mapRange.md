# mapRange

Maps a number from one range onto another. `mapRange(x, [0, 300], [0, 1])` gives 0 at 0, 0.5 at 150 and 1 at 300. Past either end it holds the end value. Add points to bend the line: `mapRange(x, [-300, 0, 300], [-20, 0, 20])`. Use it with a live value in `v-animate`.

```ts
import { mapRange } from 'vael-ui/motion' // either build
```

## Example

```ts
import { mapRange } from 'vael-ui/motion'

mapRange(150, [0, 300], [0, 1]) // 0.5
mapRange(-40, [0, 300], [0, 1]) // 0: held at the end
mapRange(450, [0, 300, 600], [1, 1, 0]) // 0.5: solid until 300, then fades

// The card tilts with the drag, every frame, with no render:
// v-animate="{ animate: { rotate: () => mapRange(drag.x.value, [-300, 300], [-20, 20]) } }"
```

## Parameters

Name | Type | Description
--- | --- | ---
`value` | `number` | The number to turn into another: an offset, a scroll position, a slider value.
`from` | `number[]` | Points along the range it comes from, in increasing order: `[0, 300]`, or more to bend the line.
`to` | `number[]` | What each of those points becomes: `to[i]` for `from[i]`. Between two points it moves in a straight line.

## Returns

Name | Type | Description
--- | --- | ---
`result` | `number` | The number in the new range. Below the first point it’s `to[0]`, above the last it’s the last of `to`.

