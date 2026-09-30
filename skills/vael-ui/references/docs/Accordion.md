# Accordion

A stack of collapsible sections where opening one can close the others.

```ts
import { Accordion } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`multiple` | `boolean \| undefined` | `false` | Lets several items be open at once; the model becomes an array.
`collapsible` | `boolean \| undefined` | `true` | Whether the last open item can close, leaving none open.
`motionCss` | `boolean \| undefined` | `true` | `false` skips transitions; use exposed `panelEl`/`open` for custom motion.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`value` | `string \| string[] \| null \| undefined` | `null` | Open item's `value`, or an array of them with `multiple`.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | The `AccordionItem`s.

## Events

Name | Type | Description
--- | --- | ---
`change` | `[value: string \| string[] \| null]` | Fires when an item opens or closes, with the new value.
`update:value` | `[value: string \| string[] \| null]` | Fires when `value` changes (`v-model:value`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.

## AccordionItem

### Props

Name | Type | Default | Description
--- | --- | --- | ---
`value` | `string` |  | Identifies the item in the parent `Accordion`'s value.
`title` | `string \| undefined` |  | Trigger text; the `#trigger` slot replaces it.
`disabled` | `boolean \| undefined` |  | Disables the trigger so the item can't be toggled.
`ui` | `Partial<{ item: UiPartValue; header: UiPartValue; trigger: UiPartValue; panel: UiPartValue; body: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

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
`open` | `boolean` | Whether the item is open. 

