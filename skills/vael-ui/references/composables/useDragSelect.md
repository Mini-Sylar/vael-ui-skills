# useDragSelect

Drag across empty space in a box and everything the band touches is selected. The box's children are the items, by their position among them, so a `v-for` list needs no selectors or attributes. Shift adds to the selection, ctrl or cmd toggles, a click on empty space clears it, and Escape during a drag puts it back. Near an edge of whatever scrolls, it scrolls and the band grows with it. A mouse or a pen draws the band; a touch still scrolls the page. It is pointer only, so pair it with checkboxes or keys for everyone else.

```ts
import { useDragSelect } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`options.el` | `MaybeRefOrGetter<HTMLElement \| null>` | The box you drag in. Its children are the items; a child with `pointer-events: none`, such as the band, isn't one.
`options.disabled` | `MaybeRefOrGetter<boolean>` | Ignores the pointer.
`options.spring` | `MaybeRefOrGetter<boolean \| Spring \| SpringName>` | `true` lets the band's moving corner trail the pointer on a spring, so it stretches and catches up: the ConfigProvider's `spring`, else the default every component uses. A name or `{ damping, response }` sets your own, and a bouncy one overshoots a little. What is selected follows the band you see. Default: the corner stays under the pointer. Reduced motion and `motion: false` keep it there.

## Returns

Name | Type | Description
--- | --- | ---
`x, y, width, height` | `Ref<number>` | The band, in px from the top left of the box, live. Draw it inside the box with `position: absolute` and `pointer-events: none`, and link its look to these with `v-animate` live values.
`stretch` | `Ref<{ x: number; y: number }>` | How far the band's moving corner is behind the pointer along each axis, in px: its tension, negative when it overshoots. Bow the band's sides in by it, as a stretched rubber band pinches. Zero without a `spring`.
`isDragging` | `Ref<boolean>` | Whether a band is showing: from the first move until it has caught up after you let go.
`selected` | `Ref<number[]>` | The items the band holds, by position among the box's children. Set it to select from code.
`clear()` | `() => void` | Selects nothing.

