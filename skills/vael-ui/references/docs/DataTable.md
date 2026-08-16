# DataTable

A data grid with sorting, selection, resizing, and virtualization, composed from Columns.

```ts
import { DataTable } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`data` | `T[]` |  | 
`rowKey` | `keyof T \| ((row: T) => string \| number)` |  | Stable row identity — a key on `T`, or a function for composite/derived keys.
`loading` | `boolean \| undefined` | false | Shows the `#loading` slot instead of rows/empty state.
`selectable` | `boolean \| undefined` | false | Adds a leading checkbox/radio column wired to the `selected` state.
`selectionMode` | `"checkbox" \| "row" \| undefined` | "checkbox" | `'checkbox'` (default): leading selection column. `'row'`: click row to toggle selection.
`single` | `boolean \| undefined` | false | Single-selection mode — `selected` holds at most one key. In `'checkbox'` mode uses `Radio`.
`scrollHeight` | `string \| undefined` |  | CSS length (`'400px'`, `'60vh'`). When set, body scrolls with sticky header; unset uses natural flow.
`stackedBreakpoint` | `string \| undefined` |  | CSS length (`'640px'`). Below this viewport width, switches to stacked card layout.
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | Row-density variant.
`stripedRows` | `boolean \| undefined` | false | Alternating row background via CSS (selected > hover > stripe precedence).
`showGridlines` | `boolean \| undefined` | false | Adds inline-end border to every cell.
`resizableColumns` | `boolean \| undefined` | false | Enables resize drag handle on every column header.
`frozenColumns` | `number \| undefined` | 0 | Freezes the first N columns sticky-left against horizontal scroll.
`rows` | `number \| undefined` |  | Rows per page. Default: all rows render. Set: internal slicing; pair with `v-model:page`. Also the page-size divisor for `lazy`'s `total`.
`manualSort` | `boolean \| undefined` | false | `data` is already sorted server-side — DataTable stops sorting it locally and only reflects `v-model:sort`, so a header click tells you what to refetch instead of re-sorting what you gave it.
`lazy` | `boolean \| undefined` | false | `data` is already just the current page — DataTable stops slicing it locally. Pair with `total` (the real across-all-pages count) so `#footer`/`Pagination` math stays correct.
`total` | `number \| undefined` |  | Real row count across all pages. Only meaningful with `lazy`; falls back to `sortedData.length` (i.e. `data.length`) when unset.
`virtualize` | `boolean \| { itemSize?: number \| undefined; overscan?: number \| undefined; estimateSize?: number \| undefined; } \| undefined` |  | Windows rendering to the visible rows + overscan, for very large `data`. Requires `scrollHeight`. `true` measures each row's real height (rows may vary, e.g. wrapping `#cell` content or `stackedBreakpoint`); pass an object to tune it.
`page` | `number \| undefined` | 1 | 
`sort` | `{ field: keyof T \| null; dir: "asc" \| "desc" \| null; } \| undefined` | { field: null, dir: null } | Uncontrolled by default (works exactly as before). Bind `v-model:sort` — required with `manualSort` — to see every header click and know what to refetch.

## Slots

Name | Type | Description
--- | --- | ---
`columns` | `{ Column: TypedColumn; columnData: T[]; }` | Declare `<Column>` children. `columnData` is the table's `:data`, handed back for type-inference.
`toolbar` | `{ selected: Set<string \| number>; count: number; }` | Toolbar content (search, bulk actions, …).
`loading` | `any` | Replaces row area while `loading` is true.
`empty` | `any` | Replaces row area when data is empty and not loading.
`footer` | `{ data: T[]; page: number; pageCount: number; total: number; }` | Footer content (pagination, …). `data` is sorted (not paginated); `page`/`pageCount` are always provided.
`expansion` | `{ row: T; }` | Full-width row beneath an expanded row. In stacked mode, always renders (no toggle).

## Events

Name | Type | Description
--- | --- | ---
`reach-end` | `[]` | 
`update:selection` | `[rows: T[]]` | 
`row-click` | `[row: T]` | 
`reach-start` | `[]` | 
`update:page` | `[value: number]` | 
`update:sort` | `[value: { field: keyof T \| null; dir: "asc" \| "desc" \| null; }]` | 

## Exposed

_None._

