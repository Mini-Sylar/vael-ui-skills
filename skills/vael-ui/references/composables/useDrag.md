# useDrag

Drags an element and decides where it goes when you let go. It follows the pointer with its own `translate`. `onRelease` receives its offset, its speed and where the throw would land. It returns where the element goes, or nothing to send it home. The element moves on a spring that keeps the throw speed, and Escape puts it back. A drag has no keyboard form, so add a button that calls `throwTo`.

```ts
import { useDrag } from 'vael-ui' // or 'vael-ui/vapor'
```

## Which one?

- `useDrag`: use it to move one element freely and decide where it lands when you let go.
- `v-draggable` or `useSortable`: use them to reorder the children of a container.

## Parameters

Name | Type | Description
--- | --- | ---
`options.el` | `MaybeRefOrGetter<HTMLElement \| null>` | The element you drag. It moves by its own `translate`, so nothing else is restyled.
`options.handle` | `string` | A CSS selector for the grab area inside `el`, such as a dialog's header: only a press there drags. Give that area `touch-action: none`. Default: all of `el`.
`options.axis` | `MaybeRefOrGetter<'x' \| 'y' \| 'both'>` | `'x'`, `'y'` or `'both'` (default). With one axis, a drag across it stays the page's.
`options.spring` | `MaybeRefOrGetter<Spring \| SpringName>` | The spring it settles on: a name or `{ damping, response }`. Default: the ConfigProvider's `spring`, else the default every component uses.
`options.disabled` | `MaybeRefOrGetter<boolean>` | Ignores the pointer, for a drag that applies only some of the time.
`options.onRelease` | `(release) => { x, y } \| null` | Runs on release with `offset`, `velocity` (px/s) and `projected`, where the throw would land. Return where it goes in px, or `null` for home.
`options.onRest` | `(offset: { x, y }) => void` | Runs when it comes to rest. Remove a thrown card here.

## Returns

Name | Type | Description
--- | --- | ---
`x, y` | `Ref<number>` | Distance from home, in px. Read them in a `v-animate` getter to follow them without re-rendering.
`isDragging` | `Ref<boolean>` | Whether a pointer is dragging it.
`throwTo(to, { velocity? })` | `(to: { x, y }, options?) => void` | Sends it to `to` on a spring, as if thrown. Use it for a button that does the swipe.
`reset({ animate? })` | `(options?: { animate?: boolean }) => void` | Sends it home. `animate: false` jumps.

