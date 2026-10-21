# useTabIndicator

The sliding selection indicator behind Tabs, SelectButton, Pagination and MenuList, and behind the active option in Select, Combobox and CommandPalette. It measures the active element inside a list and returns a style that places an indicator element on it. When the active element changes, the indicator glides there. `useHoverIndicator` builds the hover highlight on the same measurement. Use it for your own segmented control, nav or list: put the indicator element inside the list, give the list `position: relative`, and bind the `style`. It measures again when the list resizes, when web fonts finish loading, and when an ancestor finishes a CSS transition, such as a dialog scaling in. It also undoes an ancestor's scale, so the px values match the layout.

```ts
import { useTabIndicator } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`active` | `WatchSource<unknown>` | What moves the indicator, such as the selected value. It measures after each change, once the DOM shows it. For a list that also filters, pass a getter of both: `() => [activeIndex.value, items.value]`.
`listEl` | `Ref<HTMLElement \| null>` | The list. The indicator element goes inside it, and it is the indicator's containing block. A ResizeObserver on it measures again when its size changes.
`orientation` | `MaybeRefOrGetter<'horizontal' \| 'vertical'>` | Default `'horizontal'`, which sets the inline position and size. `'vertical'` sets the block position and size.
`selector` | `MaybeRefOrGetter<string>` | Default `'[role="tab"][aria-selected="true"]'`. The first match inside `listEl` is the active element. SelectButton uses `'[data-checked]'`, and Pagination uses `'[aria-current="page"]'`. With no match, the style sets `opacity: 0`.
`sizing` | `MaybeRefOrGetter<'bounds' \| 'transform'>` | Default `'bounds'`: real position and size in px, so a border radius stays true at any size. `'transform'` uses `translate` and `scale` on a 1 px baseline instead. It is cheaper, but a radius on the scaled axis flattens, and it can look misaligned at fractional pixel ratios. Use it only for an indicator with square ends, such as an underline. `scroller`, `matchCross`, `maxGlide` and a spring need `'bounds'`.
`scroller` | `boolean` | `listEl` scrolls, and the indicator sits inside its scrolled content. Positions include the scroll offset, so the indicator scrolls with the rows.
`matchCross` | `boolean` | Also sets the cross axis from the active element: top and height in a horizontal list, start and width in a vertical one. Use it when rows are inset by the list's padding. Without it, your CSS sets the cross axis.
`maxGlide` | `MaybeRefOrGetter<number \| undefined>` | A move longer than this many of the active element's own lengths jumps instead of gliding. That suits typeahead, Home and End, and a filtered list. With it set, a list that loses its match shows the next one in place instead of sliding in. Unset, every move glides.
`motion` | `MaybeRefOrGetter<boolean \| undefined>` | `false` moves the indicator at once on every change, so you can animate it yourself. Unset, it moves the way `indicator` says.
`indicator` | `MaybeRefOrGetter<'glide' \| 'instant' \| { damping?: number; response?: number } \| undefined>` | How it moves: `'glide'`, a CSS transition, `'instant'`, or a spring (see `useSpringValue`). Unset, it follows the nearest ConfigProvider's `indicator`, else `'glide'`. A spring needs `indicatorEl`. It jumps instead of springing on its first appearance, past `maxGlide`, and under reduced motion.
`indicatorEl` | `Ref<HTMLElement \| null>` | The indicator element. A spring writes position and size straight to its inline style each frame, with no re-render.

## Returns

Name | Type | Description
--- | --- | ---
`style` | `Ref<Record<string, string \| undefined>>` | Bind it on the indicator element. It holds the position and size in px, and `opacity: 0` while nothing matches. On first paint, on first appearance and past `maxGlide`, it sets the position and size durations to `0ms`, so the indicator doesn't grow in from the edge. Those overrides go slot by slot, so list your transition as position, size, then opacity, the order the `ui-indicator` classes use. With a spring, it holds only `transition-property: opacity`.
