# usePopoverService

The imperative engine behind `<Popover>` for anchored popovers opened from code instead of markup. `openPopover(Component, options)` mounts any component inside a Popover anchored to `options.triggerEl` (required, since an imperative popover has no inline `#trigger` slot to derive it from), and returns a `result` promise that settles with whatever the opened component’s `usePopoverRef().close(result)` passes. This is the low-level primitive `confirmAction({ surface: 'popover' })` is built on; reach for it directly for anything beyond a plain confirm.

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`openPopover(component, options)` | `OpenPopoverHandle<T>` | Mounts component inside <Popover>, rendered by the app-level <PopoverHost/>. options.props is typed against the component you pass; options.triggerEl is required.
`usePopoverRef()` | `PopoverRef<D, T>` | Called from inside the opened component. { data, panelEl, close(result) }: close() is what settles the opener’s result promise.
`usePopoverQueue()` | `DynamicPopoverEntry[]` | The live queue <PopoverHost/> renders. Mount PopoverHost once at the app root, alongside DialogHost.

