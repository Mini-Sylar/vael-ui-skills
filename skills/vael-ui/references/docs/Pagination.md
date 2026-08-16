# Pagination

Page-number controls for splitting a long list or table across pages.

```ts
import { Pagination } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`total` | `number` |  | Total item count across all pages (not current page's row count).
`pageSizeOptions` | `number[] \| undefined` |  | Page-size `<Select>` options. Omitted hides the dropdown.
`siblingCount` | `number \| undefined` | 1 | Page-number buttons to show on each side of current page before ellipsis.
`ui` | `Partial<{ root: UiPartValue; list: UiPartValue; button: UiPartValue; ellipsis: UiPartValue; sizeSelect: UiPartValue; }> \| undefined` |  | 
`page` | `number \| undefined` | 1 | 
`pageSize` | `number \| undefined` | 10 | 

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`update:page` | `[value: number]` | 
`update:pageSize` | `[value: number]` | 

## Exposed

_None._

