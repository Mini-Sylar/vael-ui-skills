# FileUpload

A drop zone and file list for picking files, with validation and progress.

```ts
import { FileUpload } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`accept` | `string \| undefined` |  | Native `accept` syntax: `.pdf`, `image/png`, `image/*`, comma-separated.
`capture` | `boolean \| "user" \| "environment" \| undefined` |  | Native `capture`: opens the camera instead of the picker on mobile. `'user'`/`'environment'` pick the facing side; `true` lets the browser choose.
`multiple` | `boolean \| undefined` | `true` | Accepts several files that add to the list. When `false`, a new file replaces the current one.
`maxSize` | `number \| undefined` |  | Maximum size per file, in bytes. Larger files fire `@reject` with `'maxSize'`.
`maxFiles` | `number \| undefined` |  | Maximum number of files in the list. Files past it fire `@reject` with `'maxFiles'`.
`disabled` | `boolean \| undefined` | `false` | Disables the dropzone, browse button and file input.
`name` | `string \| undefined` |  | Native `name` on the file input.
`motionCss` | `boolean \| undefined` | `true` | `false` skips the built-in file-row transition; use `@item-enter`/`@item-leave` for custom motion.
`ui` | `Partial<{ root: UiPartValue; dropzone: UiPartValue; browse: UiPartValue; list: UiPartValue; item: UiPartValue; remove: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`files` | `File[] \| undefined` | `[]` | Accepted files.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ isDragOver: boolean; browse: () => void; }` | Custom dropzone content; the dropzone keeps its drag-and-drop handling. Receives `isDragOver` and `browse`.
`item` | `{ file: File; remove: () => void; index: number; }` | Replaces one file row's content; the library keeps the `<li>`.

## Events

Name | Type | Description
--- | --- | ---
`remove` | `[file: File]` | Fires when you remove a file from the list.
`update:files` | `[value: File[]]` | Fires when `files` changes (`v-model:files`).
`reject` | `[{ file: File; reason: FileRejectReason; }]` | Fires for each file that `accept`, `maxSize` or `maxFiles` rejects, with the reason.
`add` | `[files: File[]]` | Fires with the files accepted from a pick or drop.
`item-enter` | `[el: Element, done: () => void]` | Fires when a file row enters, with the `(el, done)` transition hook. Only when `motionCss` is `false`.
`item-leave` | `[el: Element, done: () => void]` | Same as `item-enter`, for a row's exit.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`dropzoneEl` | `HTMLElement \| null` | Dropzone element.
`inputEl` | `HTMLInputElement \| null` | Native file input.
`listEl` | `HTMLElement \| null` | File list element (`null` until the first file is added).
`browse` | `() => void` | Opens the native file picker (no-op while disabled). 

