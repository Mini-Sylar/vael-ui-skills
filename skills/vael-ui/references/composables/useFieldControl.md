# useFieldControl

Connects a custom form control to the nearest `<Field>`. It returns the id and the ARIA values to bind (`aria-describedby`, `aria-invalid`, `aria-required`), and reports focus and filled state to Field. Field uses that state to move a floating label and set `data-filled`. Every built-in input (Input, Select, Checkbox, RadioGroup, …) uses it. Use it so a custom control works with Field the same way.

```ts
import { useFieldControl } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`filled` | `MaybeRefOrGetter<boolean>` | `true` when the control has a value. Reported to the nearest Field, including programmatic `v-model` writes.

## Returns

Name | Type | Description
--- | --- | ---
`id` | `string` | Bind to the control's `id`. It's Field's `controlId` when there is one, otherwise a new `useId()`.
`describedBy` | `() => string \| undefined` | Bind to `aria-describedby`.
`labelledBy` | `() => string \| undefined` | Bind to `aria-labelledby` on group controls (RadioGroup) that have no single native input.
`invalid` | `() => boolean` | OR it into the control's own `invalid` prop.
`required` | `() => boolean` | Bind to `aria-required`.
`disabled` | `() => boolean` | Advisory. OR it into the control's own `disabled` prop.
`onFocus` | `() => void` | Call it from the control's native `focus` handler.
`onBlur` | `() => void` | Call it from the control's native `blur` handler.
