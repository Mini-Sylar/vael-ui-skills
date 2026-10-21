# useScrollLock

Locks scrolling on an element, `document.body` by default. Dialog, Drawer, BottomSheet and Tour use it while they are open. It sets `overflow: hidden` and `overscroll-behavior: contain`. It also pads the element's inline end by the scrollbar's width, so the layout doesn't shift when the scrollbar goes. That width is also set as `--ui-scrollbar-width`, on `<html>` for a body lock, so a fixed header can add the same padding. Locks are counted per element: when two dialogs lock the body, it stays locked until both release. On release, the inline styles from before the lock come back, unless something else changed them meanwhile. On iOS, a body lock also blocks touch scrolling, except inside a scrollable region within the body.

```ts
import { useScrollLock } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`target` | `MaybeRefOrGetter<HTMLElement \| null>` | Default `document.body`. When it changes while locked, the lock moves to the new element. `null` releases it.
`initial` | `boolean` | Default `false`. `true` locks at once.

## Returns

Name | Type | Description
--- | --- | ---
`locked` | `WritableComputedRef<boolean>` | The return value is the ref itself: `const locked = useScrollLock()`. Set it to `true` to lock and `false` to release. The lock also releases when the calling scope ends, such as on unmount.
