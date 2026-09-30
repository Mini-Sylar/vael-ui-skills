# DataTable

A data grid with sorting, selection, resizing, and virtualization, composed from Columns.

```ts
import { DataTable } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`data` | `T[]` |  | Row objects. Each `<Column>` reads its content from them by `field`.
`rowKey` | `keyof T \| ((row: T) => string \| number)` |  | Stable row identity: a key on `T`, or a function for composite or derived keys.
`loading` | `boolean \| undefined` | `false` | Shows the `#loading` slot instead of the rows or empty state.
`selectable` | `boolean \| undefined` | `false` | Adds a leading checkbox or radio column that tracks the selection.
`selectionMode` | `"checkbox" \| "row" \| undefined` | `'checkbox'` | `'checkbox'` adds a leading selection column. `'row'` toggles selection when you click a row; the rows are then one Tab stop, arrow keys move between them, and Space or Enter toggles.
`single` | `boolean \| undefined` | `false` | Limits selection to one row. In `'checkbox'` mode, the selection column renders Radio buttons.
`scrollHeight` | `string \| undefined` |  | CSS length, such as `'400px'` or `'60vh'`. When set, the body scrolls under a sticky header.
`stackedBreakpoint` | `string \| undefined` |  | CSS length, such as `'640px'`. Below this viewport width, rows switch to a stacked card layout.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Row-density variant.
`stripedRows` | `boolean \| undefined` | `false` | Alternates row backgrounds. Selected and hover backgrounds win over stripes.
`showGridlines` | `boolean \| undefined` | `false` | Adds an inline-end border to every cell.
`resizableColumns` | `boolean \| undefined` | `false` | Adds a drag handle to every column header's edge for resizing.
`frozenColumns` | `number \| undefined` | `0` | Pins the first N columns to the left while the table scrolls horizontally.
`rows` | `number \| undefined` |  | Rows per page. Unset, all rows render. Set, the table slices rows; pair it with `v-model:page`. With `lazy`, it also divides `total` into pages.
`manualSort` | `boolean \| undefined` | `false` | Marks `data` as sorted on the server. DataTable stops sorting locally and only updates `v-model:sort`. A header click then tells you what to refetch.
`lazy` | `boolean \| undefined` | `false` | Marks `data` as the current page only, so DataTable stops slicing it. Pair it with `total` so `#footer` and Pagination math stays correct.
`total` | `number \| undefined` |  | Row count across all pages. Only used with `lazy`; unset, it falls back to `data.length`.
`virtualize` | `boolean \| { itemSize?: number \| undefined; overscan?: number \| undefined; estimateSize?: number \| undefined; } \| undefined` |  | Renders only the visible rows, for large `data`. Requires `scrollHeight`. `true` measures each row's height; pass an object to tune it (`itemSize` fixes row height).
`motionCss` | `boolean \| undefined` | `true` | Plays the built-in row enter, exit, reorder and column-drag transitions. Set `false` to animate rows yourself via `@row-enter` and `@row-leave`. Rows never animate while virtualized.
`reorderableColumns` | `boolean \| undefined` | `false` | Lets you drag column headers to reorder them. Pair with `v-model:columnOrder` to control or persist the order.
`columnGripVisibility` | `"hover" \| "always" \| undefined` | `'always'` | When a reorderable column's drag grip shows: `'always'`, or only on hover and focus (`'hover'`).
`canDrop` | `((details: SortableDropDetails) => boolean) \| undefined` |  | Runs while a column drags; return `false` to mark the target invalid. Pinned columns stay in place whatever it returns.
`beforeDrop` | `((details: SortableDropDetails) => boolean \| Promise<boolean>) \| undefined` |  | Async check at drop time for a column reorder. Return `false`, or a promise of `false`, to cancel. Pair it with `confirmAction().result` to confirm before the move.
`previewMode` | `"element" \| "clone" \| undefined` | `'clone'` | What follows the pointer during a column drag. `'clone'` floats a copy of the header. Avoid `'element'`: it can drop a column from the DOM here.
`touchDragDelay` | `number \| undefined` | `150` | Milliseconds a touch must hold a column header before a drag starts, so taps still sort. Mouse and pen drags start without the delay.
`ui` | `Partial<{ root: UiPartValue; toolbar: UiPartValue; table: UiPartValue; thead: UiPartValue; th: UiPartValue; sortButton: UiPartValue; grip: UiPartValue; resizeHandle: UiPartValue; tbody: UiPartValue; tr: UiPartValue; td: UiPartValue; expansionRow: UiPartValue; expansionContent: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`page` | `number \| undefined` | `1` | Current page, 1-based. Only used when `rows` is set; DataTable clamps it to the page count.
`sort` | `{ field: keyof T \| null; dir: "asc" \| "desc" \| null; } \| undefined` | `{ field: null, dir: null }` | Current sort field and direction. Bind it with `manualSort` to know what to refetch.
`columnOrder` | `(keyof T)[] \| undefined` | `[]` | Column `field`s in display order. When empty, columns follow their `<Column>` order in the template.

## Slots

Name | Type | Description
--- | --- | ---
`columns` | `{ Column: TypedColumn; columnData: T[]; }` | Declares `<Column>` children. `columnData` is the table's `:data`, passed back for type inference.
`toolbar` | `{ selected: Set<string \| number>; count: number; }` | Toolbar content (search, bulk actions, …).
`loading` | `any` | Replaces the row area while `loading` is true.
`empty` | `any` | Replaces the row area when `data` is empty and not loading.
`footer` | `{ data: T[]; page: number; pageCount: number; total: number; }` | Footer content (pagination, …). `data` is sorted, not paginated; the slot always receives `page` and `pageCount`.
`expansion` | `{ row: T; }` | Full-width row beneath an expanded row. In stacked mode, it always renders, with no toggle.

## Events

Name | Type | Description
--- | --- | ---
`reach-end` | `[]` | Virtualized only: fires when the rendered window nears the end of `data`, so you can fetch the next page.
`update:selection` | `[rows: T[]]` | Fires when the selection changes, with the resolved row objects (not raw keys).
`row-click` | `[row: T]` | Fires when you click a row anywhere outside its interactive descendants.
`reach-start` | `[]` | Virtualized only: fires when the rendered window nears the start of `data`, so you can fetch the previous page.
`column-reorder` | `[order: (keyof T)[]]` | Fires when a column drag commits, with the new `field` order.
`row-enter` | `[el: Element, done: () => void]` | Fires when a row enters. Call `done()` when finished; set `motionCss` to `false` to own the animation.
`row-leave` | `[el: Element, done: () => void]` | Fires when a row leaves. Call `done()` when finished, as with `@row-enter`.
`drop-error` | `[error: unknown, details: SortableDropDetails]` | Fires when `beforeDrop` throws or rejects during a column reorder, after DataTable reverts the move.
`update:page` | `[value: number]` | Fires when `page` changes (`v-model:page`).
`update:sort` | `[value: { field: keyof T \| null; dir: "asc" \| "desc" \| null; }]` | Fires when `sort` changes (`v-model:sort`).
`update:columnOrder` | `[value: (keyof T)[]]` | Fires when `columnOrder` changes (`v-model:columnOrder`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.

## Column

### Props

Name | Type | Default | Description
--- | --- | --- | ---
`field` | `keyof T` |  | Row key this column reads its values from.
`label` | `string \| undefined` |  | Header text. Falls back to `field`.
`sortable` | `boolean \| undefined` |  | Makes the header a button that cycles ascending, descending and unsorted.
`width` | `string \| number \| undefined` |  | Column width as a CSS length. Numbers are pixels.
`resizable` | `boolean \| undefined` |  | Unset, inherits DataTable's `resizableColumns`; `true` or `false` overrides it for this column.
`reorderable` | `boolean \| undefined` |  | Unset, inherits DataTable's `reorderableColumns`; `false` pins this column in place.
`data` | `T[] \| undefined` |  | Type-inference anchor only. Bind it (`<Column :data="items">`) so other props infer against `T`.

### Slots

Name | Type | Description
--- | --- | ---
`cell` | `{ row: T; value: T[keyof T]; }` | Custom cell content for each row.
`header` | `{ column: RegisteredColumn<T>; }` | Replaces the header content, including the sort button of a `sortable` column.

### Events

_None._

### Exposed

_None._

