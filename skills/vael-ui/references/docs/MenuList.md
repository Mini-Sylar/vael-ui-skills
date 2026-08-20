# MenuList

The same row rendering as Menu, but always in-flow, built for a permanent sidebar nav.

```ts
import { MenuList } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly MenuEntry<T>[] \| undefined` |  | Same shape as `Menu`'s own `items` — a `MenuList` and a `Menu` can share one array.
`active` | `string \| number \| null \| undefined` |  | The current page's `value` — renders `aria-current="page"` on the matching row.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; separator: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; }` | Override one row's content while keeping its behavior. Fires for selectable rows and group labels.

## Events

Name | Type | Description
--- | --- | ---
`select` | `[item: T]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.

