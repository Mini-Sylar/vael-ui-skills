# useVirtualizer

Renders only the visible rows of a long list, plus a few extra rows past each edge (overscan). `Select`, `Combobox` and `DataTable` use it for large lists. Use it to do the same for a scrolling list of your own.

```ts
import { useVirtualizer } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`containerEl` | `Ref<HTMLElement \| null>` | The scroll container.
`count` | `MaybeRefOrGetter<number>` | The total number of rows.
`itemSize` | `MaybeRefOrGetter<number \| undefined>` | The row size in px. Omit it to measure the first rendered row, with a 36 px estimate until then.
`overscan` | `MaybeRefOrGetter<number>` | The extra rows rendered past each edge. Default `8`.
`onReachEnd` | `() => void` | Fires once when the rendered window's last index reaches `count - 1 - overscan`. Fires again after `count` changes.

## Returns

Name | Type | Description
--- | --- | ---
`listStyle` | `Ref<Record<string, string>>` | Gives the container its full scroll height. Bind it to a relatively positioned spacer inside the container.
`items` | `Readonly<Ref<VirtualRow[]>>` | The rows to render now: `{ index, start, size, style }`.
`scrollToIndex` | `(index, align?: 'nearest' \| 'start' \| 'end' \| 'center') => void` | Scrolls a row into view. Default `'nearest'`, which suits keyboard navigation: it doesn't scroll when the row is already visible.
`measuredSize` | `Readonly<Ref<number \| null>>` | The resolved row size.

