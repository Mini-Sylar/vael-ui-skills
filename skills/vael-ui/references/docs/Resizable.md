# Resizable

A draggable handle for letting the user resize a panel between a min and max size.

```ts
import { Resizable } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` | `0` | Smallest size in pixels.
`max` | `number \| undefined` | `Infinity` | Largest size in pixels.
`direction` | `'horizontal' \| 'vertical' \| undefined` | `'horizontal'` | Axis the panel resizes along: `'horizontal'` sets its width, `'vertical'` its height.
`edge` | `'start' \| 'end' \| 'both' \| undefined` | `'end'` | Which edge the handle sits on. `'start'` flips the drag so moving toward the panel grows it. `'both'` puts a handle on each edge, and pulling one moves only that side: the far end stays put. Add `v-model:offset` to give the start a position of your own, for a span.
`centered` | `boolean \| undefined` | `false` | Resizes from the middle: the handle you pull follows the pointer and the opposite edge moves as far the other way, so the panel's centre stays put.
`bounds` | `[number, number] \| undefined` |  | With `offset`: the span stays between these, start and end, in pixels.
`scrollEl` | `HTMLElement \| null \| undefined` |  | A scrolling container: near its edges it scrolls under the pointer during a drag, and the drag follows. Its `scroll-padding` marks what covers it, such as a sticky column.
`movable` | `boolean \| undefined` | `true with `offset`` | With `offset`: press on the panel and drag to move the whole span, and the arrow keys move it a step. A still press stays a click. Set `false` to resize only.
`step` | `number \| undefined` |  | Snaps the size to a multiple of `step` on release, the drag itself staying smooth, and an arrow key resizes one step. Unset, a drag doesn't snap and an arrow key moves 16 px.
`disabled` | `boolean \| undefined` | `false` | Disables dragging and keyboard resizing, and removes the handle from the tab order.
`ariaLabel` | `string \| undefined` | `'Resize'` | Accessible label for the handle. With `edge="both"` the labels are the messages' `start` and `end`.
`motion` | `ResizableMotion \| undefined` |  | How it moves: its appearance (`{ initial: { root: { opacity: 0 } } }`) and its states (`{ states: { dragging: { handle: { scale: '1.2' } } } }`). `false` turns its own animation off.
`ui` | `Partial<{ root: UiPartValue; handle: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`size` | `number` |  | Panel size in pixels along `direction`. Overshoots `min` or `max` elastically mid-drag, then settles within them.
`offset` | `number \| undefined` |  | Where a span starts, in pixels along `direction`, for a bar on a timeline: bind it and the panel positions itself from this start (give its container `position: relative`). A handle on the start edge moves the start and the size together, so the far end stays put. Bind it from the start; it isn't read as a span if it begins unset.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Panel content.
`handle` | `{ edge: ResizeEdge; }` | Custom content inside a drag handle; `edge` says which when there are two.

## Events

Name | Type | Description
--- | --- | ---
`update:size` | `[value: number]` | Fires when `size` changes (`v-model:size`).
`update:offset` | `[value: number \| undefined]` | Fires when `offset` changes (`v-model:offset`).
`commit` | `[]` | Fires once per settled interaction: on pointer release, or after each key press. Save here instead of on every drag frame.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`handleEl` | `HTMLElement \| null` | Drag handle element.
`isDragging` | `boolean` | Whether you're dragging the handle with a pointer.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `ResizableMotion` (exported from `'vael-ui'`); `false` turns it off. Resizable: its appearance and states.

- Parts: `root`, `handle`
- States: `dragging`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

