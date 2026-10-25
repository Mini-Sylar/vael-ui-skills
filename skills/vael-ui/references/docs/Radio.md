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
`motion` | `RadioMotion \| undefined` | `true` | How it moves: its appearance (`{ initial: { control: { opacity: 0, scale: 0.8 } } }`) and its states, like `checked` or `invalid` (`{ states: { checked: { control: { scale: '1.1' } } } }`). `false` turns its own animation off, built-in transitions too; animate the exposed `controlEl`/`dotEl` yourself.
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
`controlEl` | `HTMLElement \| null` | The circle that fills when selected.
`dotEl` | `HTMLElement \| null` | The dot inside it.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `RadioMotion` (exported from `'vael-ui'`); `false` turns it off. Radio: its appearance and states.

- Parts: `root`, `control`, `label`, `description`
- States: `checked`, `unchecked`, `invalid`, `disabled`, `focus`, `active`, `hover`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.

