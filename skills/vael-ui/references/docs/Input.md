# Input

A single-line text field with sizes, states, and slots for icons or hints.

```ts
import { Input } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`type` | `string \| undefined` | `'text'` | Native input `type`.
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Control size.
`disabled` | `boolean \| undefined` | `false` | Disables the input and blocks interaction. A disabled parent Field also disables it.
`readonly` | `boolean \| undefined` | `false` | Makes the value read-only while keeping it focusable and selectable.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`placeholder` | `string \| undefined` |  | Text shown while the input is empty.
`clearable` | `boolean \| undefined` | `false` | Shows a button that empties the input while it has text.
`animated` | `boolean \| undefined` | `false` | Crossfades a placeholder or value that changes on its own, like AnimatedText. What you type never animates.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; start: UiPartValue; end: UiPartValue; clear: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `InputMotion \| undefined` |  | Animates its states, like a shake when it turns invalid: `{ states: { invalid: { root: { x: [0, -6, 6, -3, 0] } } } }`.
`modelValue` | `string \| undefined` | `''` | Input value. Supports the `.trim` and `.lazy` modifiers.

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Inline leading content, such as an icon, a Kbd hint or a copy Button.
`end` | `any` | Inline trailing content.
`display` | `{ value: string; placeholder: string; focused: boolean; }` | Draws the text yourself, over the input's own text box and following its scroll, for a fancy input: characters that animate in, gradients, highlighted tokens. The input underneath keeps the caret, selection and screen reader text; its own text is hidden. Give an element `pointer-events: auto` to let it take hover, say for a popover on a mention.

## Events

Name | Type | Description
--- | --- | ---
`clear` | `[]` | Fires when the clear button empties the input.
`update:modelValue` | `[value: string]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element (the frame around the input).
`inputEl` | `HTMLInputElement \| null` | Native `<input>` element.

## Motion

`motion` takes `InputMotion` (exported from `'vael-ui'`); `false` turns it off. Input's `motion`: values or hooks for its parts (InputPart), its states (`invalid`, `disabled`, `focus`).

- Parts: `root`, `input`, `start`, `end`
- States: `invalid`, `disabled`, `focus`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.

