# Tag

A compact label for categorizing or annotating an item inline.

```ts
import { Tag } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`variant` | `"primary" \| "danger" \| "muted" \| "success" \| "warning" \| "info" \| undefined` | `'muted'` | Color variant.
`size` | `"sm" \| "md" \| undefined` | `'md'` | Text size and padding.
`pill` | `boolean \| undefined` | `false` | Fully pill-rounded instead of the default small label corners.
`ui` | `Partial<{ root: UiPartValue; icon: UiPartValue; label: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `TagMotion \| undefined` |  | How it moves: its appearance (`{ initial: { root: { opacity: 0, scale: 0.9 } } }`) and its press states (`{ states: { active: { root: { scale: '0.96' } } } }`). `false` turns its own animation off.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Tag label.
`icon` | `any` | Small leading glyph, such as a dot or checkmark, sized to match the text rather than a full icon box.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `TagMotion` (exported from `'vael-ui'`); `false` turns it off. Tag: its appearance and press states.

- Parts: `root`
- States: `focus`, `active`, `hover`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

