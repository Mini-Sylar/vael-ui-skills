# Switch

An on/off toggle styled like a physical switch, for immediate-effect settings.

```ts
import { Switch } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`label` | `string \| undefined` |  | Label text; the default slot replaces it.
`disabled` | `boolean \| undefined` | `false` | Disables the switch and blocks interaction.
`invalid` | `boolean \| undefined` | `false` | Shows the invalid state. A surrounding `Field` in error sets it too.
`size` | `"sm" \| "md" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | Native `name` for form submission.
`motion` | `SwitchMotion \| undefined` | `true` | How it moves: its appearance (`{ initial: { thumb: { opacity: 0, scale: 0.6 } } }`) and its states, like `checked`, `dragging` or `invalid` (`{ states: { checked: { thumb: { scale: '1.1' } } } }`). `false` turns its own animation off, built-in transitions too; animate the exposed `trackEl`/`thumbEl` yourself.
`ui` | `Partial<{ root: UiPartValue; track: UiPartValue; thumb: UiPartValue; label: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `boolean \| undefined` | `false` | Whether the switch is on.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Label content; replaces the `label` text.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: boolean]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`inputEl` | `HTMLInputElement \| null` | Native checkbox input (`role="switch"`).
`trackEl` | `HTMLElement \| null` | Track element.
`thumbEl` | `HTMLElement \| null` | Thumb element.
`dragging` | `boolean` | Whether the thumb is being dragged.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `SwitchMotion` (exported from `'vael-ui'`); `false` turns it off. Switch: its appearance and states.

- Parts: `root`, `track`, `thumb`
- States: `checked`, `unchecked`, `invalid`, `disabled`, `focus`, `active`, `hover`, `dragging`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.
