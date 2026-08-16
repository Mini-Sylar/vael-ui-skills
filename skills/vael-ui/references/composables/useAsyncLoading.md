# useAsyncLoading

Tracks every in-flight promise passed to `run()`. `loading` only clears once ALL of them have settled, so overlapping calls (or several buttons sharing one instance) never flicker the state early. This is exactly what `Button`’s own `loading="auto"` uses internally for promise-returning `@click` handlers.

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`loading` | `ComputedRef<boolean>` | True while at least one call to run() hasn’t settled yet.
`run` | `<T>(fn: () => T \| Promise<T>) => Promise<T>` | Wraps fn, incrementing/decrementing the in-flight count around it.

