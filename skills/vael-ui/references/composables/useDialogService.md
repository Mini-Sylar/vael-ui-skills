# useDialogService

Opens dialogs from code instead of markup, on the same engine as `<Dialog>`. `openDialog(Component, options)` mounts any component as the dialog body, with typed props. It returns a `result` promise that settles with the value the opened component passes to `useDialogRef().close(result)`. `confirmAction()` is built on it. Use it directly for anything beyond a plain confirm, such as a rename form or a multi-step flow.

```ts
import { openDialog, useDialogRef, useDialogQueue } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`openDialog(component, options)` | `OpenDialogHandle<T>` | Mounts `component` inside `<Dialog>`, rendered by the app-level `<DialogHost/>`. `options.props` is typed against the component you pass. The handle has `result`, `close`, and the `panelEl` and `overlayEl` refs for your own animations.
`useDialogRef()` | `DialogRef<D, T>` | Call it inside the opened component. Returns `{ data, panelEl, overlayEl, close(result) }`. Calling `close()` settles the opener's `result` promise.
`useDialogQueue()` | `DynamicDialogEntry[]` | The live queue that `<DialogHost/>` renders. Mount `DialogHost` once at the app root.
