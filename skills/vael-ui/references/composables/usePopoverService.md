# usePopoverService

Opens anchored popovers from code instead of markup, with the same behavior as `<Popover>`. `openPopover(Component, options)` mounts any component inside a Popover anchored to `options.triggerEl`. `triggerEl` is required, because a popover opened from code has no `#trigger` slot. Its `result` promise settles with the value the opened component passes to `usePopoverRef().close(result)`. `confirmAction({ surface: 'popover' })` is built on it. Use it for anything beyond a plain confirm.

```ts
import { openPopover, usePopoverRef, usePopoverQueue } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`openPopover(component, options)` | `OpenPopoverHandle<T>` | Mounts `component` inside `<Popover>`, rendered by the app-level `<PopoverHost/>`. `options.props` is typed against the component you pass. `options.triggerEl` is required.
`usePopoverRef()` | `PopoverRef<D, T>` | Reaches the popover from inside the opened component: `{ data, panelEl, close(result) }`. `close()` settles the opener's `result` promise.
`usePopoverQueue()` | `DynamicPopoverEntry[]` | Returns the live queue that `<PopoverHost/>` renders. Mount `PopoverHost` once at the app root, next to `DialogHost`.

