# useHoverIndicator

The sliding hover highlight behind every vael-ui list: Select, Menu, Tree, DataTable, Tabs, Pagination, Calendar and the rest. It slides one indicator element to the row under a mouse or pen, or to the row with keyboard focus, which always wins. Touch is ignored, as hover styles are. Use it to give your own list the same feel: mark the rows with `itemSelector`, put an element with the `ui-indicator` class inside the list, and bind the `style` it returns. The list needs `position: relative; isolation: isolate`, so the indicator sits behind the rows. Style it with `--ui-indicator-bg`, `--ui-duration-indicator` and `--ui-ease-indicator`, or set `motion: false` and drive it yourself.

```ts
import { useHoverIndicator } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`listEl` | `Ref<HTMLElement \| null>` | The list. The indicator element goes inside it and moves in its coordinates.
`itemSelector` | `string` | Matches the rows the indicator moves between. It skips disabled rows.
`scope` | `string` | Keeps nested lists apart: a row counts only when its nearest match for this selector is `listEl` itself, so a submenu or a table in an expanded row keeps its own rows.
`orientation` | `MaybeRefOrGetter<'vertical' \| 'horizontal'>` | The axis it moves along. Default `'vertical'`. For a row of buttons, use `'horizontal'` with the `ui-indicator--inline` class; for a grid, add `ui-indicator--grid` to glide on both axes.
`scroller` | `boolean` | Set it when `listEl` scrolls, so the indicator scrolls with the rows inside it.
`matchCross` | `boolean` | Also matches each row across the list, for rows inset by its padding. Default `true`.
`maxGlide` | `number` | A keyboard move longer than this many rows, such as typeahead, Home or End, jumps instead of gliding. The pointer always glides. Default `3`.
`enabled` | `MaybeRefOrGetter<boolean>` | `false` hides the indicator and stops tracking, such as while a row is being dragged.
`motion` | `MaybeRefOrGetter<boolean \| undefined>` | `false` moves the indicator at once, for an animation of your own. Unset, it follows the nearest ConfigProvider's `motion`.
`indicator` | `MaybeRefOrGetter<'glide' \| 'instant' \| { damping?: number; response?: number } \| undefined>` | How it moves: a CSS glide, an instant move, or a spring. Unset, it follows the nearest ConfigProvider's `indicator`, else `'glide'`. See `useTabIndicator`.
`indicatorEl` | `Ref<HTMLElement \| null>` | The indicator element. A spring writes to it directly each frame. Without it, a spring falls back to the glide.
`group` | `string` | Passes the highlight between lists with the same `group`, so moving from one into the next glides across instead of fading out and in. MenuList and Tree use `'menu-list'` and `'tree'`.

## Returns

Name | Type | Description
--- | --- | ---
`style` | `Ref<Record<string, string \| undefined>>` | The position and size in px, to bind on the indicator element, with `opacity: 0` while nothing is hovered or focused. The highlighted row also gets `data-indicated`.

