# Timeline

An ordered list of steps connected by a line, agnostic to what each step actually contains.

```ts
import { Timeline } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | 
`orientation` | `"horizontal" \| "vertical" \| undefined` | "vertical" | 
`motionCss` | `boolean \| undefined` | true | Gates the connector fill transition and the item enter/leave/move animation.
`pulse` | `boolean \| undefined` | false | Opt-in continuous "live" pulse ring on the active marker. Off by default — a looping animation is a real stylistic commitment this component shouldn't make for you.
`itemKey` | `((item: T, index: number) => string \| number) \| undefined` | index | Identifies an item across re-renders for TransitionGroup — defaults to its index, so pass this whenever items can be reordered/spliced (not just appended) or it'll re-key wrong.
`completed` | `((item: T, index: number) => boolean) \| undefined` |  | Whether a step counts as "done" for the built-in dot fill and connector fill — the only two things Timeline itself renders any state for. Omit for a plain, uncolored list; a richer status vocabulary (success/failure/skipped, whatever) is still fully expressible — just read your own `item` inside #marker/#item and render it yourself, no help from Timeline needed. Takes priority over `current` when both are given.
`active` | `((item: T, index: number) => boolean) \| undefined` |  | Whether a step is "the current one" — drives the marker's active ring (and, with `pulse`, its live pulse). Independent of `completed`: a step can be both, neither, or either. Takes priority over `current` when both are given.
`current` | `number \| undefined` |  | Convenience for the common case where progress genuinely is one linear index: steps before it read as completed, the one at it as active. Plain number, not a v-model — Timeline never advances it itself (no built-in click-to-select), so there's nothing for it to write back. Bind your own ref and advance it however fits your UI (a button, a click handler, an action inside the active step's own content). Ignored per-flag once `completed`/`active` are given.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; opposite: UiPartValue; marker: UiPartValue; connector: UiPartValue; content: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`opposite` | `{ item: T; index: number; completed: boolean; active: boolean; }` | The other side of the line from #item — a date, a status pill, whatever. Omit it entirely and the row collapses back to the plain marker+content layout, no reserved empty column.
`marker` | `{ item: T; index: number; completed: boolean; active: boolean; }` | 
`item` | `{ item: T; index: number; completed: boolean; active: boolean; isLast: boolean; }` | 

## Events

Name | Type | Description
--- | --- | ---
`item-enter` | `[el: Element, done: () => void]` | 
`item-leave` | `[el: Element, done: () => void]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`stepEls` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`markerEls` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`connectorEls` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.

