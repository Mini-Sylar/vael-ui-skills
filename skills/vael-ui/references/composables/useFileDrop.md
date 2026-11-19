# useFileDrop

The drag-and-drop half of FileUpload. It makes any element a drop target for files from the desktop, tracks whether a drag is over it, and passes the dropped files to `onFiles`. It counts nested `dragenter` and `dragleave` events, so moving over a child element doesn't flicker the state. It doesn't validate anything: check type, size and count yourself, as FileUpload does with `accept`, `maxSize` and `maxFiles`. It doesn't open a file picker either, so pair it with an `<input type="file">` for keyboard and touch users, who can't drag files.

```ts
import { useFileDrop } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`targetEl` | `Ref<HTMLElement \| null>` | The drop target. The listeners follow it when the ref changes.
`onFiles` | `(files: File[]) => void` | Receives every dropped file. A drop with no files, such as dragged text, doesn't call it.
`disabled` | `MaybeRefOrGetter<boolean>` | Ignores every drag event while `true`. The browser then handles a drop itself, which usually opens the file in the tab.

## Returns

Name | Type | Description
--- | --- | ---
`isDragOver` | `Ref<boolean>` | `true` while a drag with files is over the target. Other drags, such as selected text, are ignored, and turning `disabled` on clears it.

