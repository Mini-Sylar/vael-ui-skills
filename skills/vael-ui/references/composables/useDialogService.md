# useDialogService

The imperative engine behind `<Dialog>` for dialogs opened from code instead of markup. `openDialog(Component, options)` mounts any component as the body (typed props included) and returns a `result` promise that settles with whatever the opened component’s `useDialogRef().close(result)` passes. This is the low-level primitive `confirmAction()` itself is built on; reach for it directly for anything beyond a plain confirm (a rename form, a multi-step flow, …).

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`openDialog(component, options)` | `OpenDialogHandle<T>` | Mounts component inside <Dialog>, rendered by the app-level <DialogHost/>. options.props is typed against the component you pass.
`useDialogRef()` | `DialogRef<D, T>` | Called from inside the opened component. { data, panelEl, close(result) }: close() is what settles the opener’s result promise.
`useDialogQueue()` | `DynamicDialogEntry[]` | The live queue <DialogHost/> renders. Mount DialogHost once at the app root.

