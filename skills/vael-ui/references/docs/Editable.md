# Editable
## Props

Name | Type | Default | Description
--- | --- | --- | ---
`placeholder` | `string \| undefined` | `'Click to edit'` | Shown in the preview when the text is empty, and in the input.
`activationMode` | `"focus" \| "click" \| "dblclick" \| undefined` | `'click'` | What opens the input: a click, a double click, or focus arriving.
`submitMode` | `"enter" \| "blur" \| "none" \| "both" \| undefined` | `'both'` | What saves the edit: Enter, leaving the input, either, or only `submit()`. Leaving the input otherwise cancels.
`selectOnFocus` | `boolean \| undefined` | `true` | Selects the text when the input opens, so typing replaces it.
`maxlength` | `number \| undefined` |  | Native `maxlength` on the input.
`disabled` | `boolean \| undefined` | `false` | Blocks editing.
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Control size, as on Input.
`motion` | `EditableMotion \| undefined` | `true` | How it moves: its appearance (`{ initial: { root: { opacity: 0, y: 4 } } }`) and its states (`{ states: { editing: { root: { scale: '1.01' } } } }`). `false` skips the swap transition; use the exposed `previewEl` and `inputEl` for your own.
`ui` | `Partial<{ root: UiPartValue; preview: UiPartValue; edit: UiPartValue; input: UiPartValue; actions: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| undefined` | `''` | The text. Updates when an edit is submitted, not on every keystroke.
`editing` | `boolean \| undefined` | `false` | Whether the input is showing. Bind it to open or close the edit from outside.

## Slots

Name | Type | Description
--- | --- | ---
`preview` | `{ value: string; }` | The resting text.
`edit` | `{ value: string; update: (value: string) => void; submit: () => void; cancel: () => void; size: "sm" \| "md" \| "lg"; }` | The control while editing, in place of the Input: a Textarea, a Select, a date picker. Bind `value` and call `update` with each change; Enter, Escape and focus leaving still work.
`actions` | `{ submit: () => void; cancel: () => void; }` | Buttons beside the control, such as save and cancel.

## Events

Name | Type | Description
--- | --- | ---
`submit` | `[value: string]` | Fires with the saved text.
`update:modelValue` | `[value: string]` | Fires when `modelValue` changes (`v-model`).
`update:editing` | `[value: boolean]` | Fires when `editing` changes (`v-model:editing`).
`cancel` | `[]` | Fires when an edit is dropped.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`previewEl` | `HTMLButtonElement \| null` | The resting text's button; `null` while editing.
`inputEl` | `HTMLInputElement \| null` | The native input; `null` until editing.
`start` | `() => void` | Opens the input.
`submit` | `() => void` | Saves the draft and closes.
`cancel` | `() => void` | Drops the draft and closes.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `EditableMotion` (exported from `'vael-ui'`); `false` turns it off. Editable: its appearance and states.

- Parts: `root`, `preview`, `edit`, `input`, `actions`
- States: `editing`, `disabled`, `focus`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.

