# Field

Wraps a form control with a label, help text, and error message, wired together for you.

```ts
import { Field } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`label` | `string \| undefined` |  | Label text, linked to the wrapped control.
`description` | `string \| undefined` |  | Help text below the control, linked via `aria-describedby`.
`error` | `string \| undefined` |  | Error message; renders with `role="alert"`.
`required` | `boolean \| undefined` |  | Shows a required marker and sets `aria-required` on the wrapped control.
`disabled` | `boolean \| undefined` |  | Disables the wrapped control.
`labelPlacement` | `"top" \| "float" \| "inset" \| undefined` | `'top'` | `'top'` stacks above the control; `'float'` overlays its edge, moving up on focus or fill; `'inset'` sits inside it.
`ui` | `Partial<{ root: UiPartValue; label: UiPartValue; control: UiPartValue; description: UiPartValue; error: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | The form control that the field labels and describes.
`label` | `any` | Replaces the label text, keeping the `<label>` element and its `for` wiring.
`description` | `any` | Custom description content; replaces the `description` text.
`error` | `{ error: string; }` | Custom error content; renders only while `error` is set.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element. 

