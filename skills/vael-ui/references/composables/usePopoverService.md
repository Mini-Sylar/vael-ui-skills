# usePopoverService

Opens anchored popovers from code instead of markup, on the same engine as `<Popover>`. `openPopover(Component, options)` mounts any component inside a Popover anchored to `options.triggerEl`. `triggerEl` is required, because an imperative popover has no `#trigger` slot to anchor to. It returns a `result` promise that settles with the value the opened component passes to `usePopoverRef().close(result)`. `confirmAction({ surface: 'popover' })` is built on it. Use it directly for anything beyond a plain confirm.

```ts
import { openPopover, usePopoverRef, usePopoverQueue } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`openPopover(component, options)` | `OpenPopoverHandle<T>` | Mounts `component` inside `<Popover>`, rendered by the app-level `<PopoverHost/>`. `options.props` is typed against the component you pass. `options.triggerEl` is required.
`usePopoverRef()` | `PopoverRef<D, T>` | Call it inside the opened component. Returns `{ data, panelEl, close(result) }`. Calling `close()` settles the opener's `result` promise.
`usePopoverQueue()` | `DynamicPopoverEntry[]` | The live queue that `<PopoverHost/>` renders. Mount `PopoverHost` once at the app root, next to `DialogHost`.
