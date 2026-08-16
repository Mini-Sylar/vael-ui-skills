# Radio

One option in a mutually-exclusive set, always used inside a RadioGroup.

```ts
import { Radio } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`value` | `string \| number` |  | 
`label` | `string \| undefined` |  | Overridden entirely by the `#default` scoped slot.
`disabled` | `boolean \| undefined` |  | 
`description` | `string \| undefined` |  | Secondary line under the label.
`ui` | `Partial<{ root: UiPartValue; control: UiPartValue; label: UiPartValue; description: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ checked: boolean; }` | 

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`inputEl` | `HTMLInputElement \| null` | 

