# Dock

A macOS-style row of icons that magnify as the pointer approaches them.

```ts
import { Dock } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `DockItemData[]` |  | Items to show, one button each.
`orientation` | `'horizontal' \| 'vertical' \| undefined` | `'horizontal'` | Lays items out in a row or a column; arrow keys follow the same axis.
`baseSize` | `number \| undefined` | `48` | Resting icon size, in pixels.
`maxSize` | `number \| undefined` | `76` | Icon size, in pixels, directly under the pointer.
`range` | `number \| undefined` |  | Falloff distance in pixels. Unset, it's 3.5 times `baseSize`.
`disabled` | `boolean \| undefined` | `false` | Dims the dock and blocks pointer interaction and magnification.
`magnify` | `boolean \| undefined` | `true` | `false` turns off magnification but keeps items interactive, unlike `disabled`.
`grow` | `boolean \| undefined` | `false` | Magnifies by resizing items, so the dock grows with them. Off, items scale and spread with transforms and the dock keeps its size.
`tooltips` | `boolean \| undefined` | `true` | Renders each item's `v-tooltip` on hover.
`tooltipSide` | `'top' \| 'right' \| 'bottom' \| 'left' \| undefined` |  | Which side each item's tooltip opens on. Unset, it's `'top'` when horizontal and `'right'` when vertical.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `DockMotion \| undefined` |  | How it moves: its appearance, item by item (`{ initial: { item: { opacity: 0, y: 8 } }, stagger: 0.04 }`), and the `spring` magnification follows the pointer on (`{ spring: 'bouncy' }`). `false` turns its own animation off.

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`select` | `[item: DockItemData, index: number]` | Fires when you click an item, or press Enter or Space on a focused item.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`itemEls` | `() => HTMLButtonElement[]` | The item buttons, in order, to animate yourself (with `:magnify="false"` to turn the built-in magnification off).
`remeasure` | `() => void` | Re-measures item positions for magnification, after a layout change the dock can't detect.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `DockMotion` (exported from `'vael-ui'`); `false` turns it off. Dock: its appearance, item by item, and each item's states: `hover`, `active` (pressed), `focus`, and `selected`, which plays each time the item is picked (a launch hop). Its `spring` is the one magnification follows the pointer on, overshoot included.

- Parts: `root`, `item`
- States: `hover`, `active`, `focus`, `disabled`, `selected`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `onAfterEnter`, `stagger`

See the [Motion guide](../guides/motion.md) for what each key does.

