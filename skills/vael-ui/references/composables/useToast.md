# useToast

Sonner-style toasts you trigger from code. Call `toast(title, options)` from anywhere, with no component context. `toast.success`, `.error`, `.warning`, `.info` and `.loading` set the variant for you. `toast.promise(input, messages)` shows a loading toast at once, then swaps it for a success or error toast when the promise settles. You don't need to call `dismiss()` yourself. Toasts render in a `<Toaster/>`, which you mount once at the app root.

```ts
import { toast, useToastQueue } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`toast(title, options?)` | `number \| string` | Shows a toast with the default variant and returns its id for `dismiss(id)`: a number, or your own `id` if you pass one. Options: `id`, `description`, `variant`, `duration` (in milliseconds, default 4000; `Infinity` keeps it until someone closes it), `action: { label, onClick }` `pinned`, which shows the toast in its own section where newer toasts can't cover it, and `toaster`, which sends it to the `<Toaster>` with that `name` instead of the unnamed one. Showing a toast with an `id` that's already on screen updates that toast instead of adding another.
`toast.success / .error / .warning / .info / .loading` | `(title, options?) => number` | Same signature with a fixed variant. `loading` defaults to `duration: Infinity`.
`toast.promise(input, messages, options?)` | `Promise<T>` | `input` is a promise or a function that returns one. Shows `messages.loading` at once, then `messages.success` or `messages.error`. Each can be a string or a function of the settled value or error.
`toast.dismiss(id?)` | `(id?: number \| string) => void` | Dismisses one toast, or every toast when you omit `id`.
`useToastQueue(toaster?)` | `{ toasts, dismiss, pauseAll, resumeAll, setWaiting }` | Read-only access to one Toaster's queue, plus pause and resume: the unnamed one, or the one whose `name` you pass. `<Toaster/>` uses it internally, for example to pause timers on `pointerenter`. `setWaiting(ids)` holds the timers of toasts that aren't on screen yet.
