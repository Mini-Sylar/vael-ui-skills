# Skeleton

A placeholder shape that mimics real content while it is still loading.

```ts
import { Skeleton } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`variant` | `"text" \| "rect" \| "circle" \| undefined` | `'text'` | `'text'`: a `1em`-tall rounded line. `'circle'`: round, with `aspect-ratio: 1`. `'rect'`: `--ui-radius` corners; content or `ui.root` sets its size.
`animated` | `boolean \| undefined` | `true` | Shows the shimmer animation.
`motion` | `SkeletonMotion \| undefined` |  | How it moves: its appearance (`{ initial: { root: { opacity: 0 } } }`). `false` turns its appearance and the pulse off.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Placeholder content that sizes the skeleton; it renders hidden.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `SkeletonMotion` (exported from `'vael-ui'`); `false` turns it off. Skeleton: its appearance; `false` holds the pulse still.

- Parts: `root`
- Keys: `spring`, `duration`, `easing`, `initial`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

