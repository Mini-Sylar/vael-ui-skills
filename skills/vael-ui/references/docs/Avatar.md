# Avatar

A circular (or square) image or initials, representing a person or entity.

```ts
import { Avatar } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`src` | `string \| undefined` |  | Image URL. Falls back to the default slot or initials until it loads, or if it fails.
`alt` | `string \| undefined` |  | Image alt text; defaults to `name`.
`name` | `string \| undefined` |  | Person's name, used for the fallback initials and as the default `alt`.
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Avatar size.
`shape` | `"circle" \| "square" \| undefined` | `'circle'` | Round or rounded-square frame.
`badgePlacement` | `"top-start" \| "top-end" \| "bottom-start" \| "bottom-end" \| undefined` | `'bottom-end'` | Which corner the `#badge` slot sits on.
`motion` | `AvatarMotion \| undefined` |  | How it moves: its parts' appearance, like the image fading in over the fallback (`{ initial: { image: { opacity: 0 } } }`). `false` turns its own animation off.
`ui` | `Partial<{ root: UiPartValue; image: UiPartValue; fallback: UiPartValue; badge: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Fallback content shown while there's no loaded image; defaults to the initials of `name`.
`badge` | `any` | Overlay on the avatar's edge, e.g. a `Badge`; position it with `badgePlacement`.

## Events

Name | Type | Description
--- | --- | ---
`load` | `[event: Event]` | Fires when the image loads.
`error` | `[event: Event]` | Fires when the image fails to load.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`imgEl` | `HTMLImageElement \| null` | Image element (null without `src`).
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `AvatarMotion` (exported from `'vael-ui'`); `false` turns it off. Avatar: its parts' appearance, like the image fading in over the fallback.

- Parts: `root`, `image`, `fallback`, `badge`
- Keys: `spring`, `duration`, `easing`, `initial`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

