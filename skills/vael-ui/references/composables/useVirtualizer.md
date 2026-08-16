# useVirtualizer

Windowed rendering for long lists: only the visible rows (plus overscan) exist in the DOM. This is what `Select`/`Combobox`/`Tree` reach for once their list gets large; use it directly when building a custom scrollable list that needs the same treatment.

## Parameters

Name | Type | Description
--- | --- | ---
`containerEl` | `Ref<HTMLElement \| null>` | The scrollable box.
`count` | `MaybeRefOrGetter<number>` | Total row count.
`itemSize` | `MaybeRefOrGetter<number \| undefined>` | Row size in px. Omit to auto-measure the first rendered row (36px estimate until then).
`overscan` | `MaybeRefOrGetter<number>` | Extra rows rendered past each edge. Default 8.
`onReachEnd` | `() => void` | Fires once the rendered window nears count - 1; re-arms when count changes.

## Returns

Name | Type | Description
--- | --- | ---
`listStyle` | `Ref<Record<string, string>>` | Bind to a relative, full-height spacer: gives the container real scrollable height.
`items` | `Readonly<Ref<VirtualRow[]>>` | The currently rendered window: { index, start, style }.
`scrollToIndex` | `(index, align?: 'nearest' \| 'start' \| 'end' \| 'center') => void` | Default 'nearest': what keyboard nav wants, never move a row that's already visible.
`measuredSize` | `Readonly<Ref<number \| null>>` | The resolved per-row size.

