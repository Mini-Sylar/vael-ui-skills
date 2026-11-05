# useScrollLength

How far the page, or a scrolling container, can scroll, in px, as a ref. It follows the content growing or shrinking, as rows loading in, and the window resizing. Pair it with `useScrollProgress` to turn progress into a distance: `progress.value * length.value`.

```ts
import { useScrollLength } from 'vael-ui/motion' // either build
```

## Parameters

Name | Type | Description
--- | --- | ---
`container` | `MaybeRefOrGetter<Element \| null \| undefined>` | A scrolling element. Leave it out for the page.
`options.axis` | `'x' \| 'y'` | Which way it scrolls. Default `'y'`.

## Returns

Name | Type | Description
--- | --- | ---
`length` | `Readonly<Ref<number>>` | The scrollable distance in px: the content past what shows. `0` on the server.

