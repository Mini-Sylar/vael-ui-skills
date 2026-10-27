# Badge

A small dot, count, or label overlaid on the corner of another element.

```ts
import { Badge } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`variant` | `"primary" \| "danger" \| "muted" \| "success" \| "warning" \| "info" \| undefined` | `'primary'` | Color variant.
`count` | `number \| undefined` |  | Number to display; capped by `max`.
`max` | `number \| undefined` | `99` | Counts above this render as `"${max}+"`.
`dot` | `boolean \| undefined` | `false` | Minimal size with no content: a plain presence dot.
`animated` | `boolean \| undefined` | `true` | `false` drops the built-in count-change animation so you can drive your own.
`motion` | `BadgeMotion \| undefined` |  | How it moves: its appearance (`{ initial: { root: { opacity: 0, scale: 0.6 } } }`). `false` turns its own appearance off; `animated` keeps the count change.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Overrides `count` entirely; anything you render here wins.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `BadgeMotion` (exported from `'vael-ui'`); `false` turns it off. Badge: its appearance.

- Parts: `root`
- Keys: `spring`, `duration`, `easing`, `initial`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

