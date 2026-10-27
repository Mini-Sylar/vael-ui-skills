# MenuList

The same row rendering as Menu, but always in-flow, built for a permanent sidebar nav.

```ts
import { MenuList } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly MenuEntry<T>[] \| undefined` |  | Data-driven rows, in the same shape as Menu's `items`, so a MenuList and a Menu can share one array.
`active` | `string \| number \| null \| undefined` |  | `value` of the current page; the matching row gets `aria-current="page"`.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; separator: UiPartValue; icon: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `MenuListMotion \| undefined` |  | How it moves: its appearance, row by row (`{ initial: { item: { opacity: 0, x: -4 } }, stagger: 0.03 }`), over `root`, `item` and the current page's `indicator`. `spring` retimes the indicator; `move` hands its moves to your own animation. `false` turns its own animation off.

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; isGroup: boolean; }` | Override one row's content while keeping its behavior. `isGroup` marks an inert group label (an entry with `items`, whose children render beneath it).

## Events

Name | Type | Description
--- | --- | ---
`select` | `[item: T]` | Fires on click, or Enter/Space when the row has roving focus.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`indicatorEl` | `HTMLElement \| null` | The current page's indicator, sliding between rows (null when no row is the current page).
`highlightEl` | `HTMLElement \| null` | The hover and keyboard highlight, sliding between rows. Under a ConfigProvider with `motion: false` it moves at once, for your own animation.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `MenuListMotion` (exported from `'vael-ui'`); `false` turns it off. MenuList: its appearance, and how the current-page indicator moves.

- Parts: `root`, `item`, `indicator`
- Keys: `spring`, `duration`, `easing`, `initial`, `move`, `replay`, `appear`, `onAfterEnter`, `stagger`

See the [Motion guide](../guides/motion.md) for what each key does.

