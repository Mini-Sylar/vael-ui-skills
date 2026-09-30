# OtpInput

A row of single-character boxes for one-time codes, with paste and auto-advance.

```ts
import { OtpInput } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`length` | `number \| undefined` | `6` | Number of cells, and the maximum code length.
`type` | `"numeric" \| "alphanumeric" \| undefined` | `'numeric'` | Restricts input to digits or to alphanumeric characters.
`mask` | `boolean \| undefined` | `false` | Shows a bullet instead of the entered character in every filled cell.
`disabled` | `boolean \| undefined` | `false` | Disables the input and blocks interaction. A disabled parent Field also disables it.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | Native `name` on the input, for plain `<form>` submission.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; cell: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| undefined` | `''` | Entered code.

## Slots

Name | Type | Description
--- | --- | ---
`cell` | `{ char: string \| null; index: number; active: boolean; filled: boolean; }` | Custom content for one cell.

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

