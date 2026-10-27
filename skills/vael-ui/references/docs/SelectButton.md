# SelectButton

A segmented control for choosing one (or several) values from a short list.

```ts
import { SelectButton } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Options to choose from.
`multiple` | `boolean \| undefined` | `false` | Turns on multi-select; the model becomes an array.
`allowEmpty` | `boolean \| undefined` | `true` | Single mode only: clicking the active option clears the model.
`disabled` | `boolean \| undefined` | `false` | Disables every option and blocks interaction.
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | Native `name` for form submission. Auto-generated when omitted.
`ui` | `Partial<{ root: UiPartValue; option: UiPartValue; indicator: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `SelectButtonMotion \| undefined` |  | How it moves: its appearance, option by option (`{ initial: { option: { opacity: 0, y: 4 } }, stagger: 0.03 }`), over `root`, `option` and the selection `indicator`, and its states (`{ states: { disabled: { root: { opacity: 0.6 } } } }`). `spring` retimes the indicator; `move` hands its moves to your own animation. `false` turns its own animation off.
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | `null` | Selected value, an array of values with `multiple`, or `null` when nothing is selected.

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; checked: boolean; }` | Custom content for each option. Receives `item` and `checked`.

## Events

Name | Type | Description
--- | --- | ---
`change` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when you change the selection, with the new model value.
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`indicatorEl` | `HTMLElement \| null` | Sliding selection indicator (single mode only; `null` with `multiple`).
`highlightEl` | `HTMLElement \| null` | The hover and keyboard highlight, sliding between options. Under a ConfigProvider with `motion: false` it moves at once, for your own animation.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `SelectButtonMotion` (exported from `'vael-ui'`); `false` turns it off. SelectButton: its appearance, and how the selection moves.

- Parts: `root`, `option`, `indicator`
- States: `disabled`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `move`, `stretch`, `squash`, `replay`, `appear`, `onAfterEnter`, `stagger`

See the [Motion guide](../guides/motion.md) for what each key does.

