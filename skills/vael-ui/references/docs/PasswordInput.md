# PasswordInput

A password field with a reveal toggle and a requirements hint.

```ts
import { PasswordInput } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Control size.
`disabled` | `boolean \| undefined` | `false` | Disables the input and reveal toggle. A disabled parent Field also disables it.
`readonly` | `boolean \| undefined` | `false` | Makes the value read-only while keeping it focusable and selectable.
`invalid` | `boolean \| undefined` | `false` | Standalone override; ORed with the nearest Field's `error` state.
`placeholder` | `string \| undefined` |  | Text shown while the input is empty.
`name` | `string \| undefined` |  | Native `name` on the input, for plain `<form>` submission.
`autocomplete` | `string \| undefined` |  | `'current-password'` for a login form, `'new-password'` for signup, reset or change forms. Set it yourself; only the screen's context determines which one fits.
`revealable` | `boolean \| undefined` | `true` | `false` hides the reveal toggle, for forms that must never show the password in plain text.
`rules` | `PasswordRule[] \| undefined` |  | Requirement checks (`{ label, test }`) for the hint checklist and the `#hint` slot's `results`. There are no built-in rules; without `rules` or `#hint`, the hint doesn't render.
`hintPlacement` | `"inline" \| "none" \| "popover" \| undefined` | `'popover'` | Where the requirements hint renders. `'none'` disables it outright.
`motionCss` | `boolean \| undefined` | `true` | `false` skips the hint's enter/exit transition. Inline mode only.
`side` | `Side \| undefined` |  | Which side of the input the hint appears on, in both `'popover'` and `'inline'` mode. Inline defaults to `'bottom'`.
`align` | `Align \| undefined` |  | How the hint aligns against the input along that side, in both `'popover'` and `'inline'` mode. Inline defaults to `'start'`.
`sideOffset` | `number \| undefined` |  | Gap between the input and the hint popover, in pixels. Popover mode only.
`alignOffset` | `number \| undefined` |  | Shifts the hint popover along the alignment axis, in pixels. Popover mode only.
`teleportTo` | `string \| HTMLElement \| undefined` |  | Teleport target for the hint popover: a CSS selector or element. Popover mode only.
`forceMount` | `boolean \| undefined` | `false` | Keeps the hint popover mounted (toggled with `v-show`) and skips the built-in transition. Popover mode only.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation for the hint popover; call `done()` when it's complete. Popover mode only.
`ui` | `Partial<{ root: UiPartValue; frame: UiPartValue; input: UiPartValue; toggle: UiPartValue; hint: UiPartValue; hintList: UiPartValue; hintItem: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| undefined` | `''` | Password value.
`visible` | `boolean \| undefined` | `false` | Whether the password is shown as plain text.

## Slots

Name | Type | Description
--- | --- | ---
`end` | `any` | Inline trailing content, placed before the reveal toggle.
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

