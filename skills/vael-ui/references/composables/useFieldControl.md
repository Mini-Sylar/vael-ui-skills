# useFieldControl

Wires a custom form control into the nearest `<Field>`: id/label association, `aria-describedby`/`aria-invalid`/`aria-required`, and reporting focus/filled state so Field can move a floating label or flip `data-filled`. This is what every built-in input (Input, Select, Checkbox, RadioGroup, …) uses internally; reach for it directly when building a custom control that should plug into Field the same way.

## Parameters

Name | Type | Description
--- | --- | ---
`filled` | `MaybeRefOrGetter<boolean>` | Reactive "does this control currently have a value" signal, reported to the nearest Field, including programmatic v-model writes.

## Returns

Name | Type | Description
--- | --- | ---
`id` | `string` | Bind to the control's own id: Field's controlId when present, otherwise a fresh useId().
`describedBy` | `() => string \| undefined` | Bind to aria-describedby.
`labelledBy` | `() => string \| undefined` | Bind to aria-labelledby on group-shaped controls (RadioGroup) with no single native input.
`invalid` | `() => boolean` | OR this into the control's own invalid prop.
`required` | `() => boolean` | Bind to aria-required.
`disabled` | `() => boolean` | Advisory: OR into the control's own disabled prop.
`onFocus` | `() => void` | Call from the control's native focus handler.
`onBlur` | `() => void` | Call from the control's native blur handler.

