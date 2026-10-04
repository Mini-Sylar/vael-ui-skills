# useToast

Sonner-style imperative toasts: call `toast(title, options)` from anywhere, no component context needed. `toast.success/error/warning/info/loading` are shortcuts for each variant, and `toast.promise(input, messages)` shows a loading toast immediately and swaps it for success/error once the promise settles, with no manual dismiss() bookkeeping. Requires a `<Toaster/>` mounted once at the app root to actually render.

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`toast(title, options?)` | `number \| string` | Pushes a default-variant toast and returns its id for `dismiss(id)`: a number, or your own `id` if you pass one. Options: `id`, `description`, `variant`, `duration` (in milliseconds, default 4000; `Infinity` keeps it until someone closes it), `action: { label, onClick }` and `pinned`, which shows the toast in its own section beside the stack where newer toasts can't cover it (it doesn't count toward `maxVisible`, and it still times out unless `duration` is `Infinity`). Showing a toast with an `id` that's already on screen updates that toast and restarts its timer instead of adding another, so you don't need to track ids yourself.
`toast.success / .error / .warning / .info / .loading` | `(title, options?) => number` | Same signature, fixed variant. loading defaults to duration: Infinity.
`toast.promise(input, messages, options?)` | `Promise<T>` | input is a Promise or a function returning one. Shows messages.loading immediately, then messages.success/.error (string or a function of the settled value/error).
`toast.dismiss(id?)` | `(id?: number | string) => void` | Dismisses one toast, or every toast when id is omitted.
`useToastQueue()` | `{ toasts, dismiss, pauseAll, resumeAll, setWaiting }` | Read-only queue access plus pause/resume: what <Toaster/> itself uses internally (e.g. pausing timers on pointerenter). `setWaiting(ids)` holds the timers of toasts that aren't on screen yet.

