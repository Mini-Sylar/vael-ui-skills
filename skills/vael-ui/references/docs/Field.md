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
`labelPlacement` | `"top" \| "start" \| "end" \| "float" \| "inset" \| undefined` | `'top'` | `'top'` stacks above the control; `'float'` overlays its edge, moving up on focus or fill; `'inset'` sits inside it; `'start'`/`'end'` sit before/after it on the same line. With `'start'`/`'end'`, add `attached` to join the label to the control as one box, and `labelWidth` to line up a column of fields.
`attached` | `boolean \| undefined` | `false` | Draws a `start`/`end` label as a muted cell that shares the control's border. It stays the real `<label>`. Controls without a bordered frame (Switch, Slider…) keep a plain side label. Has no effect with `top`, `float` or `inset`.
`labelWidth` | `string \| number \| undefined` | `'max-content'` | Width of a `start`/`end` label: a number is pixels, a string any CSS length (`'8rem'`). Give fields the same width to line up their controls, or set `--ui-field-label-width` once on a parent. An `attached` label truncates with an ellipsis when its text is wider.
`labelAlign` | `"start" \| "end" \| undefined` | `'start'` | Aligns a `start`/`end` label's text. Use `'end'` for right-aligned form labels that sit next to the control.
`ui` | `Partial<{ root: UiPartValue; label: UiPartValue; control: UiPartValue; group: UiPartValue; prepend: UiPartValue; append: UiPartValue; description: UiPartValue; error: UiPartValue; }> \| undefined` |  | Class and style overrides for each part. Joined cells (an `attached` label, `#prepend`, `#append`) take their colors from `--ui-field-cell-bg` and `--ui-field-cell-color`.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | The form control that the field labels and describes.
`label` | `any` | Replaces the label text, keeping the `<label>` element and its `for` wiring.
`prepend` | `any` | Cell joined to the control's leading edge. Text and icons share one segment, and each control (a Select, a Button) gets its own, so `$` plus a currency Select reads `[ $ \| USD ]`. Put controls directly in the slot, not inside a wrapper element. The Field's label doesn't name them, so give each one an `aria-label`; the Field's `disabled` still applies. Next to a control without a frame (Switch, Slider…), the cell renders as plain text.
`append` | `any` | Cell joined to the control's trailing edge; same rules as `#prepend`.
`description` | `any` | Custom description content; replaces the `description` text.
`error` | `{ error: string; }` | Custom error content; renders only while `error` is set.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element. 

