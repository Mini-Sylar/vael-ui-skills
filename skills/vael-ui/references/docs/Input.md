# Input

A single-line text field with sizes, states, and slots for icons or hints.

```ts
import { Input } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`type` | `string \| undefined` | `'text'` | Native input `type`.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Control size.
`disabled` | `boolean \| undefined` | `false` | Disables the input and blocks interaction. A disabled parent Field also disables it.
`readonly` | `boolean \| undefined` | `false` | Makes the value read-only while keeping it focusable and selectable.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`placeholder` | `string \| undefined` |  | Text shown while the input is empty.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; start: UiPartValue; end: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| undefined` | `''` | Input value. Supports the `.trim` and `.lazy` modifiers.

## Slots

Name | Type | Description
--- | --- | ---
`start` | `any` | Inline leading content, such as an icon, a Kbd hint or a copy Button.
`end` | `any` | Inline trailing content.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element (the frame around the input).
`inputEl` | `HTMLInputElement \| null` | Native `<input>` element. 

