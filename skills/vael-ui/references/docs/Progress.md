# Progress

A bar or ring showing completion of a determinate or indeterminate task.

```ts
import { Progress } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`value` | `number \| null \| undefined` |  | `null`/`undefined` renders the indeterminate (looping) state.
`max` | `number \| undefined` | `100` | Value that counts as complete; Progress clamps `value` to `0`–`max`.
`label` | `string \| undefined` |  | Accessible label for the progress bar.
`variant` | `"primary" \| "danger" \| "success" \| "warning" \| "info" \| undefined` | `'primary'` | Fill color variant.
`size` | `"md" \| "sm" \| undefined` | `'md'` | Track thickness.
`ui` | `Partial<{ root: UiPartValue; track: UiPartValue; fill: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

_None._

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`fillEl` | `HTMLElement \| null` | Fill element; its scale comes from the `--ui-progress-scale` custom property. 

