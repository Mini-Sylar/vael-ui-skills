# Accordion

A stack of collapsible sections where opening one can close the others.

```ts
import { Accordion } from 'vael-ui' // or 'vael-ui/vapor'
```

## Notes

`headingLevel` (default `3`) sets the heading tag that wraps each item's trigger; match it to the page outline. An `AccordionItem` can override it with its own `headingLevel`.

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`multiple` | `boolean \| undefined` | `false` | Lets several items be open at once; the model becomes an array.
`collapsible` | `boolean \| undefined` | `true` | Whether the last open item can close, leaving none open.
`motion` | `AccordionMotion \| undefined` | `true` | `false` turns the transitions off, so you can animate each item's exposed `panelEl` and `chevronEl` yourself. An object animates each item's states, like a quarter-turn chevron: `{ states: { open: { chevron: { rotate: '90deg' } } } }`.
`headingLevel` | `1 \| 2 \| 3 \| 4 \| 5 \| 6 \| undefined` | `3` | Heading level wrapping each item's trigger; match it to the surrounding outline.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`value` | `V \| undefined` | `null` | Open item's `value` (a `string`, or a union of your item values), or an array of them with `multiple`. `V` takes the type you bind, and `@change` hands the same type back.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | The `AccordionItem`s.

## Events

Name | Type | Description
--- | --- | ---
`change` | `[value: V]` | Fires when an item opens or closes, with the new value.
`update:value` | `[value: V \| undefined]` | Fires when `value` changes (`v-model:value`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.

## Motion

`motion` takes `AccordionMotion` (exported from `'vael-ui'`); `false` turns it off. Accordion's `motion`: values or hooks for its parts (AccordionPart), its states (`open`, `disabled`).

- Parts: `item`, `header`, `trigger`, `chevron`, `panel`, `body`
- States: `open`, `disabled`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.

## AccordionItem

### Props

Name | Type | Default | Description
--- | --- | --- | ---
`value` | `string` |  | Identifies the item in the parent `Accordion`'s value.
`title` | `string \| undefined` |  | Trigger text; the `#trigger` slot replaces it.
`disabled` | `boolean \| undefined` |  | Disables the trigger so the item can't be toggled.
`headingLevel` | `1 \| 2 \| 3 \| 4 \| 5 \| 6 \| undefined` |  | Overrides the parent `Accordion`'s `headingLevel` for this item.
`ui` | `Partial<{ item: UiPartValue; header: UiPartValue; trigger: UiPartValue; panel: UiPartValue; body: UiPartValue; chevron: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

### Slots

Name | Type | Description
--- | --- | ---
`default` | `{ open: boolean; toggle: () => void; }` | Panel content.
`trigger` | `{ open: boolean; toggle: () => void; }` | Replaces the trigger's title and chevron; renders inside the trigger button.

### Events

_None._

### Exposed

Name | Type | Description
--- | --- | ---
`panelEl` | `HTMLElement \| null` | Panel element, always mounted.
`chevronEl` | `SVGSVGElement \| null` | The built-in chevron, for your own engine to turn. Null when the `#trigger` slot replaces it.
`open` | `boolean` | Whether the item is open.

