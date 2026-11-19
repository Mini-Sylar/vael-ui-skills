# useFieldControl

Connects a custom form control to the nearest `<Field>`. It returns the id and the ARIA values to bind (`aria-describedby`, `aria-invalid`, `aria-required`), and reports focus and filled state to Field. Field uses that state to move a floating label and set `data-filled`. Every built-in input (Input, Select, Checkbox, RadioGroup, …) uses it. Use it so a custom control works with Field the same way.

```ts
import { useFieldControl } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`filled` | `MaybeRefOrGetter<boolean>` | Tells the nearest Field whether the control has a value, including values your code sets through `v-model`.

## Returns

Name | Type | Description
--- | --- | ---
`id` | `string` | The id to bind on the control: Field's `controlId` when there is one, otherwise a new `useId()`.
`describedBy` | `() => string \| undefined` | The value for `aria-describedby`.
`labelledBy` | `() => string \| undefined` | The value for `aria-labelledby`, on group controls such as RadioGroup that have no single native input.
`invalid` | `() => boolean` | Whether Field marks the control invalid. Combine it with the control's own `invalid` prop.
`required` | `() => boolean` | The value for `aria-required`.
`disabled` | `() => boolean` | Whether Field disables the control. Combine it with the control's own `disabled` prop.
`onFocus` | `() => void` | Reports focus to Field. Call it from the control's native `focus` event.
`onBlur` | `() => void` | Reports blur to Field. Call it from the control's native `blur` event.

