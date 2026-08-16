# FileUpload

A drop zone and file list for picking files, with validation and progress.

```ts
import { FileUpload } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`accept` | `string \| undefined` |  | Native `accept` syntax: `.pdf`, `image/png`, `image/*`, comma-separated.
`multiple` | `boolean \| undefined` | true | 
`maxSize` | `number \| undefined` |  | 
`maxFiles` | `number \| undefined` |  | 
`disabled` | `boolean \| undefined` | false | 
`name` | `string \| undefined` |  | 
`ui` | `Partial<{ root: UiPartValue; dropzone: UiPartValue; browse: UiPartValue; list: UiPartValue; item: UiPartValue; remove: UiPartValue; }> \| undefined` |  | 
`files` | `File[] \| undefined` | [] | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ isDragOver: boolean; browse: () => void; }` | Replaces the dropzone's inner content; the library keeps the dropzone element + drag wiring.
`item` | `{ file: File; remove: () => void; index: number; }` | Replaces one file row's content; the library keeps the `<li>`.

## Events

Name | Type | Description
--- | --- | ---
`remove` | `[file: File]` | 
`update:files` | `[value: File[]]` | 
`reject` | `[{ file: File; reason: FileRejectReason; }]` | 
`add` | `[files: File[]]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`dropzoneEl` | `HTMLElement \| null` | 
`inputEl` | `HTMLInputElement \| null` | 
`browse` | `() => void` | 

