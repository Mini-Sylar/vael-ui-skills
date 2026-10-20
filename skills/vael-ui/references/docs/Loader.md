# Loader

A small spinner for indicating that something is in progress.

```ts
import { Loader } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `string \| undefined` |  | Any CSS length. Sets the root's `font-size`, and the `em`-based ring scales with it.
`label` | `string \| undefined` |  | Renders `role="status"` with visually hidden text as the accessible name. Omit it to make the loader `aria-hidden`.
`type` | `LoaderType \| undefined` | `'ring'` | The animation: `'ring' \| 'dots' \| 'bars' \| 'comet' \| 'dither'`. Unset, it follows the nearest ConfigProvider's `loaderType`.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

_None._

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.

Every type is pure CSS in a 1em square that follows `currentColor`, so swapping one never moves the layout. `ConfigProvider`'s `loaderType` sets the type for every spinner inside: Loader, Button's loading state, list loading rows, loading toasts and PullToRefresh. Button takes its own `loaderType` too. 

