# useScrollLock

Locks scrolling on an element, `document.body` by default. Dialog, Drawer, BottomSheet and Tour use it while they are open. It sets `overflow: hidden` and `overscroll-behavior: contain`. It also pads the element's inline end by the scrollbar's width, so the layout doesn't shift when the scrollbar goes. It sets that width as `--ui-scrollbar-width` too, on `<html>` for a body lock, so a fixed header can add the same padding. Locks are counted per element: when two dialogs lock the body, it stays locked until both release. On release, the inline styles from before the lock come back, unless something else changed them in the meantime. On iOS, a body lock also blocks touch scrolling, except inside a scrollable region within the body.

```ts
import { useScrollLock } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`target` | `MaybeRefOrGetter<HTMLElement \| null>` | The element to lock. Default `document.body`. When it changes while locked, the lock moves to the new element; `null` releases it.
`initial` | `boolean` | Locks at once when `true`. Default `false`.

## Returns

Name | Type | Description
--- | --- | ---
`locked` | `WritableComputedRef<boolean>` | Locks while `true`. The composable returns this ref itself: `const locked = useScrollLock()`. The lock also releases when the calling scope ends, such as on unmount.

