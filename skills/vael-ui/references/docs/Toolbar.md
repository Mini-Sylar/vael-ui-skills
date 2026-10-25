# Toolbar

A horizontal strip for grouping related actions, with roving keyboard focus.

```ts
import { Toolbar } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'horizontal'` | Lays the controls out in a row or a column; arrow keys follow the axis.
`overflowLabel` | `string \| undefined` | `'More'` | Accessible label for the overflow (`…`) menu button.
`ui` | `Partial<{ root: UiPartValue; group: UiPartValue; overflowTrigger: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `ToolbarMotion \| undefined` |  | How it moves: its appearance, control by control (`{ initial: { item: { opacity: 0, y: 4 } }, stagger: 0.03 }`), over `root`, `item` (each control in a group) and the hover `indicator`. `spring` retimes the indicator; `move` hands its moves to your own animation. `false` turns its own animation off.

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Controls at the start of the toolbar. Mark a child `data-toolbar-overflow` to let it collapse into the `…` menu.
`default` | `any` | Controls in the start group, after `#start`.
`center` | `any` | Controls in the center group.
`end` | `any` | Controls at the end of the toolbar, before the `…` overflow menu.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`highlightEl` | `HTMLElement \| null` | The hover and keyboard highlight, sliding between buttons. Under a ConfigProvider with `motion: false` it moves at once, for your own animation.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `ToolbarMotion` (exported from `'vael-ui'`); `false` turns it off. Toolbar: its appearance, and how the hover highlight moves.

- Parts: `root`, `item`, `indicator`
- Keys: `spring`, `duration`, `easing`, `initial`, `move`, `replay`, `appear`, `stagger`

See the [Motion guide](../guides/motion.md) for what each key does.

