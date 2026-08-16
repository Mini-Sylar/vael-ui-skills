# useToast

Sonner-style imperative toasts: call `toast(title, options)` from anywhere, no component context needed. `toast.success/error/warning/info/loading` are shortcuts for each variant, and `toast.promise(input, messages)` shows a loading toast immediately and swaps it for success/error once the promise settles, with no manual dismiss() bookkeeping. Requires a `<Toaster/>` mounted once at the app root to actually render.

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`toast(title, options?)` | `number` | Pushes a default-variant toast; returns its id (for dismiss(id)).
`toast.success / .error / .warning / .info / .loading` | `(title, options?) => number` | Same signature, fixed variant. loading defaults to duration: Infinity.
`toast.promise(input, messages, options?)` | `Promise<T>` | input is a Promise or a function returning one. Shows messages.loading immediately, then messages.success/.error (string or a function of the settled value/error).
`toast.dismiss(id?)` | `(id?: number) => void` | Dismisses one toast, or every toast when id is omitted.
`useToastQueue()` | `{ toasts, dismiss, pauseAll, resumeAll }` | Read-only queue access plus pause/resume: what <Toaster/> itself uses internally (e.g. pausing timers on pointerenter).

