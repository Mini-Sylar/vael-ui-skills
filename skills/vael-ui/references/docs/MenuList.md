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
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; separator: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

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

