# Timeline

An ordered list of steps connected by a line, agnostic to what each step actually contains.

```ts
import { Timeline } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Entries to show; any shape, rendered through the `#item` slot.
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'vertical'` | Lays the entries out in a row or a column.
`motionCss` | `boolean \| undefined` | `true` | Gates the connector fill transition and the item enter/leave/move animation.
`pulse` | `boolean \| undefined` | `false` | Adds a looping pulse ring to the active marker.
`itemKey` | `((item: T, index: number) => string \| number) \| undefined` | `(item, index) => index` | Stable key for each item. Pass it when items can be reordered or removed.
`completed` | `((item: T, index: number) => boolean) \| undefined` |  | Whether an item is done, which fills its dot and connector. Takes priority over `current`.
`active` | `((item: T, index: number) => boolean) \| undefined` |  | Whether an item is the current one, which rings its marker (and pulses it with `pulse`). Independent of `completed`; takes priority over `current`.
`current` | `number \| undefined` |  | Index of the current item: earlier items read as completed, this one as active. Not a v-model; Timeline never changes it. `completed`/`active` override it when given.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; opposite: UiPartValue; marker: UiPartValue; connector: UiPartValue; content: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`opposite` | `{ item: T; index: number; completed: boolean; active: boolean; }` | Content on the other side of the line from `#item`, such as a date. Without it, Timeline reserves no column.
`marker` | `{ item: T; index: number; completed: boolean; active: boolean; }` | Custom marker content, replacing the default dot.
`item` | `{ item: T; index: number; completed: boolean; active: boolean; isLast: boolean; }` | Content for each item; defaults to the item itself as text.

## Events

Name | Type | Description
--- | --- | ---
`item-enter` | `[el: Element, done: () => void]` | Fires when an item enters, only while `motionCss` is `false`. Run your own animation, then call `done()`.
`item-leave` | `[el: Element, done: () => void]` | Same as `@item-enter`, for an item leaving.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`stepEls` | `HTMLElement[] \| null` | Item (`<li>`) elements, in order.
`markerEls` | `HTMLElement[] \| null` | Marker elements, in order.
`connectorEls` | `HTMLElement[] \| null` | Connector elements, one fewer than items (the last item has none).

