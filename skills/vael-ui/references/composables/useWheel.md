# useWheel

Moves a position along a list of items. You drag it, throw it, scroll it or step it with the keys, and it rests on one item. It draws nothing. Each item near the resting spot gets `--ui-wheel-offset` (in items) and `--ui-wheel-distance` (in px). Place items with those variables, and style them with `v-animate` and `progressOf`. Add `role="spinbutton"` for a picker, or `aria-roledescription="carousel"` and Previous and Next buttons for a carousel. For a plain list of cards, CSS scroll snap is simpler.

```ts
import { useWheel } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`model` | `Ref<number>` | The index of the item it rests on. It changes when an item is chosen, and setting it moves the wheel.
`options.el` | `MaybeRefOrGetter<HTMLElement \| null>` | The element you drag, scroll and focus. Its children are the items, in order.
`options.count` | `MaybeRefOrGetter<number>` | How many items.
`options.size` | `MaybeRefOrGetter<number \| 'auto'>` | Item length along the axis, in px. Default `36`. `'auto'` matches the length of `el`.
`options.orientation` | `MaybeRefOrGetter<'vertical' \| 'horizontal'>` | `'vertical'` (default) or `'horizontal'`. Horizontal reverses right to left.
`options.loop` | `MaybeRefOrGetter<boolean>` | Wraps from the last item to the first.
`options.disabled` | `MaybeRefOrGetter<boolean>` | Ignores the pointer, the mouse wheel and the keys.
`options.spring` | `MaybeRefOrGetter<Spring \| SpringName>` | The spring it settles on: a name or `{ damping, response }`. Default: the ConfigProvider's `spring`, else the default every component uses.
`options.onTick` | `(index: number) => void` | Runs for each item that passes the resting spot, and once for the item it lands on. Pass `useHaptic().selection` for a click per item.
`options.onSettle` | `(index: number) => void` | Runs when it comes to rest.
`options.sizes` | `MaybeRefOrGetter<number[]>` | Item lengths by index, when they differ.
`options.gap` | `MaybeRefOrGetter<number>` | Space between items, in px. Default `0`.
`options.align` | `MaybeRefOrGetter<'start' \| 'center' \| 'end'>` | Where an item rests in `el`: `'start'`, `'center'` (default) or `'end'`.
`options.dragFree` | `MaybeRefOrGetter<boolean>` | Rests where a throw ends instead of on an item.
`options.slidesToScroll` | `MaybeRefOrGetter<number>` | Items per step. `2` moves two at a time. Default `1`.
`options.containScroll` | `MaybeRefOrGetter<boolean>` | Stops with the last item flush against the end of `el`.

## Returns

Name | Type | Description
--- | --- | ---
`position` | `Ref<number>` | Where it is now, in items. 2.5 is halfway from item 2 to item 3.
`index` | `ComputedRef<number>` | The item it rests on, or is heading for.
`isDragging` | `Ref<boolean>` | Whether a pointer is dragging it.
`goTo(i, { animate? })` | `(i: number, options?: { animate?: boolean }) => void` | Moves to item `i`. A loop takes the short way. `animate: false` jumps.
`step(n)` | `(n: number) => void` | Moves `n` steps forward, or back when negative. Use it for Previous and Next buttons.
`progressOf(i, reach?)` | `(i: number, reach?: number) => number` | Place of item `i` among the `reach` items either side of the resting spot (default 3), from 0 to 1. Pass it to `v-animate` as `progress`.
`onKeydown` | `(event: KeyboardEvent) => void` | The arrow keys along the axis, Home, End, Page Up and Page Down. Bind it to `@keydown` on `el`.

