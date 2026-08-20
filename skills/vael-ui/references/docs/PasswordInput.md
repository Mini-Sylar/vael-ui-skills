# PasswordInput

A password field with a reveal toggle and a requirements hint.

```ts
import { PasswordInput } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`disabled` | `boolean \| undefined` | false | 
`readonly` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | Standalone override; ORed with the nearest Field's `error` state.
`placeholder` | `string \| undefined` |  | 
`name` | `string \| undefined` |  | 
`autocomplete` | `string \| undefined` |  | `'current-password'` for a login form, `'new-password'` for signup/reset/change. No default: only the screen's context determines which, so set it explicitly.
`revealable` | `boolean \| undefined` | true | `false` hides the reveal toggle entirely — e.g. a compliance-sensitive form that never wants the password shown in plain text.
`rules` | `PasswordRule[] \| undefined` |  | Requirement checks driving the default hint checklist (and the `#hint` slot's `results` scope). No built-in default — the labels are user-facing text, and this component has no i18n context to translate them from, so shipping one would silently be English-only. Pass your own, e.g.: ```ts const rules: PasswordRule[] = [  { label: t('password.minLength'), test: (v) => v.length >= 8 },  { label: t('password.oneNumber'), test: (v) => /[0-9]/.test(v) }, ] ``` Omitting both `rules` and `#hint` leaves nothing to show, so the hint doesn't mount at all.
`hintPlacement` | `"inline" \| "none" \| "popover" \| undefined` | "popover" | Where the requirements hint renders. `'none'` disables it outright.
`motionCss` | `boolean \| undefined` | true | Inline mode only: gates the enter/exit transition.
`side` | `Side \| undefined` |  | Which side of the input the hint appears on. In `'popover'` mode this is floating-ui's own side, forwarded straight to the inner Popover; in `'inline'` mode it controls the flex layout instead (`'top'`/`'bottom'` stack, `'left'`/ `'right'` sit the hint beside the input). Default: `'bottom'`.
`align` | `Align \| undefined` |  | Cross-axis alignment. In `'popover'` mode this is floating-ui's own align, forwarded to the inner Popover; in `'inline'` mode it positions the hint relative to the input instead — along the input's width for `'top'`/ `'bottom'`, along its height for `'left'`/`'right'`. Default: `'start'`.
`sideOffset` | `number \| undefined` |  | Popover mode only, forwarded to the inner Popover.
`alignOffset` | `number \| undefined` |  | 
`teleportTo` | `string \| HTMLElement \| undefined` |  | 
`forceMount` | `boolean \| undefined` | false | 
`beforeClose` | `((done: () => void) => void) \| undefined` |  | 
`ui` | `Partial<{ root: UiPartValue; frame: UiPartValue; input: UiPartValue; toggle: UiPartValue; hint: UiPartValue; hintList: UiPartValue; hintItem: UiPartValue; }> \| undefined` |  | 
`modelValue` | `string \| undefined` | "" | 
`visible` | `boolean \| undefined` | false | 

## Slots

Name | Type | Description
--- | --- | ---
`end` | `any` | Inline trailing content, placed before the reveal toggle.
`hint` | `{ value: string; results: PasswordRuleResult[]; }` | Replaces the default requirements checklist. Falls back to it when omitted.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string]` | 
`update:visible` | `[value: boolean]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`inputEl` | `HTMLInputElement \| null` | 
`hintPanelEl` | `HTMLElement \| null` | 
`closeHint` | `() => void` | 
`cancelCloseHint` | `() => void` | 

