# Loader

A small spinner for indicating that something is in progress.

```ts
import { Loader } from 'vael-ui' // or 'vael-ui/vapor'
```

Every type is pure CSS in a 1em square that follows `currentColor`, so swapping one never moves the layout. `ConfigProvider`'s `loaderType` sets the type for every spinner inside: Loader, Button's loading state, list loading rows, loading toasts and PullToRefresh. Button takes its own `loaderType` too.

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `string \| undefined` |  | Any CSS length. Sets the root's `font-size`, and the `em`-based ring scales with it.
`label` | `string \| undefined` |  | Renders `role="status"` with visually hidden text as the accessible name. Omit it to make the loader `aria-hidden`.
`type` | `LoaderSource \| undefined` | `'ring'` | The animation: `'ring'`, `'dots'`, `'bars'`, `'comet'` or `'dither'`, or your own component in the same 1em box. Unset, it follows the nearest ConfigProvider's `loaderType`.
`motion` | `LoaderMotion \| undefined` |  | How it moves: its appearance (`{ initial: { root: { opacity: 0, scale: 0.6 } } }`). `false` holds it still, for a static busy mark.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

_None._

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `LoaderMotion` (exported from `'vael-ui'`); `false` turns it off. Loader: its appearance; `false` holds it still.

- Parts: `root`
- Keys: `spring`, `duration`, `easing`, `initial`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.
