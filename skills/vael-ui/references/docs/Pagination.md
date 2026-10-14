# Pagination

Page-number controls for splitting a long list or table across pages.

```ts
import { Pagination } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`total` | `number` |  | Total item count across all pages, not the current page's row count.
`pageSizeOptions` | `number[] \| undefined` |  | Page-size `<Select>` options. Omit it to hide the dropdown.
`siblingCount` | `number \| undefined` | `1` | Page-number buttons to show on each side of the current page before an ellipsis.
`motionCss` | `boolean \| undefined` | `true` | `false` skips the indicator's built-in sliding; animate the exposed `indicatorEl` and `labelsEl` yourself.
`ui` | `Partial<{ root: UiPartValue; list: UiPartValue; button: UiPartValue; ellipsis: UiPartValue; sizeSelect: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`page` | `number \| undefined` | `1` | Current page, 1-based.
`pageSize` | `number \| undefined` | `10` | Rows per page. Picking a size from the dropdown resets `page` to 1.

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`update:page` | `[value: number]` | Fires when `page` changes (`v-model:page`).
`update:pageSize` | `[value: number]` | Fires when `pageSize` changes (`v-model:pageSize`).

## Exposed

Name | Type | Description
--- | --- | ---
`listEl` | `HTMLElement \| null` | The page-number list.
`indicatorEl` | `HTMLElement \| null` | The sliding highlight behind the current page.
`labelsEl` | `HTMLElement \| null` | The page numbers in the contrast color, clipped to the indicator.

