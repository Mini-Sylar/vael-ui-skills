# Checkbox

A tri-state toggle for a single yes/no/indeterminate choice.

```ts
import { Checkbox } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`label` | `string \| undefined` |  | Label text; the default slot replaces it.
`value` | `string \| number \| undefined` |  | Value added to an array model when checked. Checked state reflects its membership.
`indeterminate` | `boolean \| undefined` | `false` | Shows the mixed (indeterminate) state, which takes precedence over checked.
`disabled` | `boolean \| undefined` | `false` | Disables the checkbox and blocks interaction.
`invalid` | `boolean \| undefined` | `false` | Shows the invalid state. A surrounding `Field` in error sets it too.
`size` | `"sm" \| "md" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | Native `name` for form submission.
`motion` | `CheckboxMotion \| undefined` | `true` | How it moves: its appearance (`{ initial: { control: { opacity: 0, scale: 0.8 } } }`) and its states, like `checked`, `indeterminate` or `invalid` (`{ states: { checked: { control: { scale: '1.1' } } } }`). `false` turns its own animation off, built-in transitions too; animate the exposed `boxEl`/`checkEl` yourself.
`ui` | `Partial<{ root: UiPartValue; box: UiPartValue; label: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `boolean \| unknown[] \| undefined` | `false` | Checked state. Bind an array to add or remove this checkbox's `value` in it (checkbox group).

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Label content; replaces the `label` text.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: boolean \| unknown[]]` | Fires when `modelValue` changes (`v-model`).
`change` | `[checked: boolean]` | Fires when you toggle the checkbox, with its new checked state.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`inputEl` | `HTMLInputElement \| null` | Native checkbox input.
`boxEl` | `HTMLElement \| null` | Visual box element.
`checkEl` | `SVGElement \| null` | Check-mark SVG element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `CheckboxMotion` (exported from `'vael-ui'`); `false` turns it off. Checkbox: its appearance and states.

- Parts: `root`, `control`, `indicator`, `label`
- States: `checked`, `unchecked`, `indeterminate`, `invalid`, `disabled`, `focus`, `active`, `hover`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

