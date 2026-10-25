# useScrollProgress

Scroll progress as a ref from 0 to 1: the page's, from top to bottom, or an element's way through the screen (or a scrolling container). Pass it to `v-animate`'s `progress` as a getter, `progress: () => scroll.value`, and keyframes follow the scroll without re-rendering the component. It measures on the next frame after a scroll or resize, and stops when its scope ends.

## Parameters

Name | Type | Description
--- | --- | ---
`target` | `MaybeRefOrGetter<Element \| null \| undefined>` | The element whose way through the screen to track. Leave it out for the page's scroll from top to bottom.
`options.container` | `MaybeRefOrGetter<Element \| null \| undefined>` | A scrolling element to measure in, instead of the page.
`options.axis` | `'x' \| 'y'` | Which way it scrolls. Default `'y'`.
`options.offset` | `[string, string]` | Where it starts and ends, each a `'<target edge> <container edge>'` pair: `start`, `center`, `end`, a share such as `0.25`, px or %. `'start end'` is the target's start meeting the container's end. Default `['start end', 'end start']`: from entering to having left.

## Returns

Name | Type | Description
--- | --- | ---
`progress` | `Readonly<Ref<number>>` | From 0 to 1, clamped. `0` on the server.

