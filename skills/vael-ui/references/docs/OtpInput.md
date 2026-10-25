# OtpInput

A row of single-character boxes for one-time codes, with paste and auto-advance.

```ts
import { OtpInput } from 'vael-ui' // or 'vael-ui/vapor'
```

## Notes

`aria-label`, `aria-labelledby` and `aria-describedby` go on the hidden native `<input>` that takes the typing, not the wrapper; a surrounding `Field` label works too.

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`length` | `number \| undefined` | `6` | Number of cells, and the maximum code length.
`type` | `"numeric" \| "alphanumeric" \| undefined` | `'numeric'` | Restricts input to digits or to alphanumeric characters.
`mask` | `boolean \| undefined` | `false` | Shows a bullet instead of the entered character in every filled cell, and keeps screen readers from reading the code.
`disabled` | `boolean \| undefined` | `false` | Disables the input and blocks interaction. A disabled parent Field also disables it.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Control size.
`variant` | `"cells" \| "field" \| undefined` | `'cells'` | `'field'` draws one box around the whole code, like a phone's verification field, instead of a box per cell.
`placeholder` | `string \| undefined` |  | One character shown in each empty cell, such as `0` or `•`.
`name` | `string \| undefined` |  | Native `name` on the input, for plain `<form>` submission.
`groups` | `number \| number[] \| undefined` |  | Splits the cells into groups with a separator between them: a group size (`3` for `123-456`), or each group's size (`[3, 4]`).
`motion` | `OtpInputMotion \| undefined` |  | Animates its states, like a shake when the code is wrong: `{ states: { invalid: { root: { x: [0, -6, 6, -3, 0] } } } }`.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; cell: UiPartValue; separator: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| undefined` | `''` | Entered code.

## Slots

Name | Type | Description
--- | --- | ---
`cell` | `{ char: string \| null; index: number; active: boolean; filled: boolean; }` | Custom content for one cell.
`separator` | `{ index: number; }` | Replaces the dash between groups. `index` counts separators from 0.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string]` | Fires when `modelValue` changes (`v-model`).
`complete` | `[code: string]` | Fires with the code each time it changes to a full `length` characters, not on every re-render.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`inputEl` | `HTMLInputElement \| null` | Invisible native `<input>` that receives typing and paste.
`cellEls` | `HTMLElement[] \| null` | Cell elements, one per character slot.
`ringEl` | `HTMLElement \| null` | The ring and caret that glide to the active cell. `null` with a custom `#cell` slot.

## Motion

`motion` takes `OtpInputMotion` (exported from `'vael-ui'`); `false` turns it off. OtpInput's `motion`: its parts (OtpInputPart) and states (`invalid`, `complete`, `disabled`, `focus`). Each cell plays them on its own, plus `filled` and `current` (the caret's); give a cell's values as a function to use its `index`.

- Parts: `root`, `cell`
- States: `invalid`, `complete`, `disabled`, `focus`, `filled`, `current`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.

