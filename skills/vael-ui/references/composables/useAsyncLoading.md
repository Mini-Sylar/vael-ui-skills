# useAsyncLoading

Tracks every in-flight promise you pass to `run()`. `loading` stays `true` until all of them settle, so overlapping calls, or several buttons sharing one instance, don't make the state flicker. `Button` uses it for `loading="auto"` when an `@click` handler returns a promise.

```ts
import { useAsyncLoading } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

_None._

## Returns

Name | Type | Description
--- | --- | ---
`loading` | `ComputedRef<boolean>` | `true` while at least one `run()` call hasn't settled.
`run` | `<T>(fn: () => T \| Promise<T>) => Promise<T>` | Runs `fn` and counts it as in flight until it settles.
