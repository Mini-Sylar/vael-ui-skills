# Radio

One option in a mutually-exclusive set, always used inside a RadioGroup.

```ts
import { Radio } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`value` | `string \| number` |  | Value the parent RadioGroup's model takes when this radio is selected.
`label` | `string \| undefined` |  | Label text; the default slot replaces it.
`disabled` | `boolean \| undefined` |  | Disables this radio. The parent RadioGroup's `disabled` also disables it.
`description` | `string \| undefined` |  | Secondary line under the label.
`ui` | `Partial<{ root: UiPartValue; control: UiPartValue; label: UiPartValue; description: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ checked: boolean; }` | Label content; replaces the `label` text. Receives `checked`.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`inputEl` | `HTMLInputElement \| null` | Native radio input. 

