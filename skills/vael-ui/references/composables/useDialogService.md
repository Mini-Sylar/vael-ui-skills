# useDialogService

Opens dialogs from code instead of markup, with the same behavior as `<Dialog>`. `openDialog(Component, options)` mounts any component as the dialog body, with typed props. Its `result` promise settles with the value the opened component passes to `useDialogRef().close(result)`. `confirmAction()` is built on it. Use it for anything beyond a plain confirm, such as a rename form or a multi-step flow.

```ts
import { openDialog, useDialogRef, useDialogQueue } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`openDialog(component, options)` | `OpenDialogHandle<T>` | Mounts `component` inside `<Dialog>`, rendered by the app-level `<DialogHost/>`. `options.props` is typed against the component you pass. The handle has `result`, `close`, and the `panelEl` and `overlayEl` refs for your own animations.
`useDialogRef()` | `DialogRef<D, T>` | Reaches the dialog from inside the opened component: `{ data, panelEl, overlayEl, close(result) }`. `close()` settles the opener's `result` promise.
`useDialogQueue()` | `DynamicDialogEntry[]` | Returns the live queue that `<DialogHost/>` renders. Mount `DialogHost` once at the app root.

