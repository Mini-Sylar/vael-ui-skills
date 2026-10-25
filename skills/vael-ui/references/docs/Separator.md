# Separator

A thin line (or gap) for dividing content into visually distinct groups.

```ts
import { Separator } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'horizontal'` | Draws a horizontal or vertical line.
`ui` | `Partial<{ root: UiPartValue; line: UiPartValue; text: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `SeparatorMotion \| undefined` |  | How it moves: its appearance, like the lines drawing out from a label (`{ initial: { line: { scale: '0 1' } } }`). `false` turns its own animation off.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Optional label centered in the line, e.g. "OR". Plain divider without it.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `SeparatorMotion` (exported from `'vael-ui'`); `false` turns it off. Separator's `motion`: values or hooks for its parts (SeparatorPart).

- Parts: `root`, `line`, `text`
- Keys: `spring`, `duration`, `easing`, `initial`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.

