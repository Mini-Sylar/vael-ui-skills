# useHoverIndicator

The sliding hover highlight behind every vael-ui list: Select, Menu, Tree, DataTable, Tabs, Pagination, Calendar and the rest. It follows the row under a mouse or pen, or the row with keyboard focus (keyboard always wins), and slides one indicator element between them. Touch is ignored, as hover styles are. Use it to give your own list the same feel: mark the rows with `itemSelector`, put an element with the `ui-indicator` class inside the list, and bind the `style` it returns. The list needs `position: relative; isolation: isolate`, so the indicator sits behind the rows. Style it with `--ui-indicator-bg`, `--ui-duration-indicator` and `--ui-ease-indicator`, or set `motion: false` and drive it yourself.

```ts
import { useHoverIndicator } from 'vael-ui' // or 'vael-ui/vapor'
```

For most lists, the `v-hover-indicator` directive does all of this in one attribute: it adds the indicator element and the positioning, and takes the same `scope`, `orientation`, `scroller`, `group` and `enabled`, plus `class` and `.over` (a tint over rows with a solid background). Reach for the composable when you need `maxGlide`, `motion`, `indicator` or your own indicator element.

```vue
<ul v-hover-indicator="'.row'">
  <li v-for="item in items" :key="item.id" class="row">{{ item.name }}</li>
</ul>
```

## Parameters

Name | Type | Description
--- | --- | ---
`listEl` | `Ref<HTMLElement \| null>` | The list. The indicator element goes inside it, and moves in its coordinates.
`itemSelector` | `string` | Matches the rows the indicator moves between. Disabled rows are skipped.
`scope` | `string` | A row counts only when its nearest match for this is `listEl` itself, so a nested list (a submenu, a table in an expanded row) keeps its own rows.
`orientation` | `MaybeRefOrGetter<'vertical' \| 'horizontal'>` | Default `'vertical'`. For a row of buttons, use `'horizontal'` with the `ui-indicator--inline` class; for a grid, add `ui-indicator--grid` to glide on both axes.
`scroller` | `boolean` | `listEl` scrolls, and the indicator scrolls with the rows inside it.
`matchCross` | `boolean` | Default `true`: also matches each row across the list, for rows inset by its padding.
`maxGlide` | `number` | Default `3`. A move longer than this many rows jumps instead of gliding.
`enabled` | `MaybeRefOrGetter<boolean>` | `false` hides the indicator and stops tracking, while a row is being dragged, say.
`motion` | `MaybeRefOrGetter<boolean \| undefined>` | `false` moves the indicator at once, for an animation of your own. Unset, it follows the nearest ConfigProvider's `motion`.
`indicator` | `MaybeRefOrGetter<'glide' \| 'instant' \| { damping?: number; response?: number } \| undefined>` | How it moves: a CSS glide, an instant move, or a spring. Unset, it follows the nearest ConfigProvider's `indicator`, else `'glide'`. See `useTabIndicator`.
`indicatorEl` | `Ref<HTMLElement \| null>` | The indicator element. A spring writes to it directly each frame. Without it, a spring falls back to the glide.

## Returns

Name | Type | Description
--- | --- | ---
`style` | `Ref<Record<string, string \| undefined>>` | Bind it on the indicator element: its position and size in px, and `opacity: 0` while nothing is hovered or focused. The hovered row also carries `data-indicated`.
