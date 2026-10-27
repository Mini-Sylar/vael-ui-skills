# Kbd

Styles a keyboard shortcut so it reads like a physical key.

```ts
import { Kbd } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`motion` | `KbdMotion \| undefined` |  | How it moves: its appearance (`{ initial: { root: { opacity: 0, y: 2 } } }`). `false` turns its own animation off.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Key or shortcut text, e.g. `⌘` or `Esc`.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `KbdMotion` (exported from `'vael-ui'`); `false` turns it off. Kbd: its appearance.

- Parts: `root`
- Keys: `spring`, `duration`, `easing`, `initial`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

