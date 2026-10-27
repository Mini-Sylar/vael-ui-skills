# PasswordInput

A password field with a reveal toggle and a requirements hint.

```ts
import { PasswordInput } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Control size.
`disabled` | `boolean \| undefined` | `false` | Disables the input and reveal toggle. A disabled parent Field also disables it.
`readonly` | `boolean \| undefined` | `false` | Makes the value read-only while keeping it focusable and selectable.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`placeholder` | `string \| undefined` |  | Text shown while the input is empty.
`name` | `string \| undefined` |  | Native `name` on the input, for plain `<form>` submission.
`autocomplete` | `string \| undefined` |  | Tells password managers what this field is. Defaults to `'new-password'` when the field shows requirements (`rules` or `#hint`), so the phone offers a strong password, and to `'current-password'` otherwise, so a saved one fills in. Set it for a case that reads otherwise, like a sign-up form without rules.
`revealable` | `boolean \| undefined` | `true` | `false` hides the reveal toggle, for forms that must never show the password in plain text.
`rules` | `PasswordRule[] \| undefined` |  | Requirement checks (`{ label, test }`) for the hint checklist and the `#hint` slot's `results`. There are no built-in rules; without `rules` or `#hint`, the hint doesn't render.
`hintPlacement` | `"inline" \| "none" \| "popover" \| undefined` | `'popover'` | Where the requirements hint renders. `'none'` disables it outright.
`motion` | `PasswordInputMotion \| undefined` | `true` | How it moves: its appearance (`{ initial: { root: { opacity: 0, y: 4 } } }`) and its states (`{ states: { invalid: { root: { x: [0, -6, 6, 0] } } } }`). The inline `hint` is a part; the popover one is not. `false` skips the inline hint's enter/exit transition.
`side` | `'top' \| 'right' \| 'bottom' \| 'left' \| undefined` |  | Which side of the input the hint appears on, in both `'popover'` and `'inline'` mode. Inline defaults to `'bottom'`.
`align` | `'start' \| 'end' \| 'center' \| undefined` |  | How the hint aligns against the input along that side, in both `'popover'` and `'inline'` mode. Inline defaults to `'start'`.
`sideOffset` | `number \| undefined` |  | Gap between the input and the hint popover, in pixels. Popover mode only.
`alignOffset` | `number \| undefined` |  | Shifts the hint popover along the alignment axis, in pixels. Popover mode only.
`teleportTo` | `string \| HTMLElement \| undefined` |  | Teleport target for the hint popover: a CSS selector or element. Popover mode only.
`forceMount` | `boolean \| undefined` | `false` | Keeps the hint popover mounted (toggled with `v-show`) and skips the built-in transition. Popover mode only.
`beforeClose` | `BeforeClose<PopoverCloseReason> \| undefined` |  | Runs before the hint closes. Return `true` to close it, `false` to keep it open, or a promise of either, like `() => confirmAction(…).result`. The hint stays open while the promise is pending. Popover mode only.
`ui` | `Partial<{ root: UiPartValue; frame: UiPartValue; input: UiPartValue; toggle: UiPartValue; hint: UiPartValue; hintList: UiPartValue; hintItem: UiPartValue; hintIcon: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| undefined` | `''` | Password value.
`visible` | `boolean \| undefined` | `false` | Whether the password is shown as plain text.

## Slots

Name | Type | Description
--- | --- | ---
`end` | `any` | Inline trailing content, placed before the reveal toggle.
`display` | `{ value: string; placeholder: string; focused: boolean; }` | Draws the text yourself over the field's own text, as Input's `#display`: the real input keeps the caret, selection and screen reader text.
`hint` | `{ value: string; results: PasswordRuleResult[]; }` | Replaces the default requirements checklist. Falls back to it when omitted.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string]` | Fires when `modelValue` changes (`v-model`).
`update:visible` | `[value: boolean]` | Fires when `visible` changes (`v-model:visible`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`inputEl` | `HTMLInputElement \| null` | Native `<input>` element.
`visible` | `boolean` | Whether the password is shown as plain text (writable).
`hintPanelEl` | `HTMLElement \| null` | Hint popover panel element (null while closed or in `'inline'` mode).
`closeHint` | `() => void` | Closes the hint popover, running `beforeClose` if set.
`cancelCloseHint` | `() => void` | Aborts a pending `beforeClose` exit and keeps the hint popover open.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `PasswordInputMotion` (exported from `'vael-ui'`); `false` turns it off. PasswordInput: its appearance and states.

- Parts: `root`, `input`, `toggle`, `hint`
- States: `invalid`, `disabled`, `focus`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

