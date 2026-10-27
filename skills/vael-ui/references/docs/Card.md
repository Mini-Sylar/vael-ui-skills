# Card

A bordered container for grouping related content, with optional title and actions.

```ts
import { Card } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`title` | `string \| undefined` |  | Default header title; the `#header` slot replaces it.
`description` | `string \| undefined` |  | Default header description; the `#header` slot replaces it.
`as` | `string \| undefined` | `'div'` | Root tag; use `'a'` or `'button'` for a fully interactive card.
`interactive` | `boolean \| undefined` | `false` | Adds a hover and press affordance. Always on when `as` is `'a'` or `'button'`.
`motion` | `CardMotion \| undefined` |  | How it moves: its parts' appearance (`{ initial: { root: { opacity: 0, y: 8 } } }`) and, on an interactive card, its press states (`{ states: { active: { root: { scale: '0.98' } } } }`). `false` turns its own animation off.
`ui` | `Partial<{ root: UiPartValue; header: UiPartValue; title: UiPartValue; description: UiPartValue; body: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Card body content.
`header` | `any` | Replaces the default title/description header.
`footer` | `any` | Footer content; the footer renders only when this slot is used.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `CardMotion` (exported from `'vael-ui'`); `false` turns it off. Card: its parts' appearance, and press states on an interactive card.

- Parts: `root`, `header`, `title`, `description`, `body`, `footer`
- States: `active`, `hover`, `focus`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

