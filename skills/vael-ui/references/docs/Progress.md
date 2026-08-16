# Progress

A bar or ring showing completion of a determinate or indeterminate task.

```ts
import { Progress } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`value` | `number \| null \| undefined` |  | `null`/`undefined` renders the indeterminate (looping) state.
`max` | `number \| undefined` | 100 | 
`label` | `string \| undefined` |  | 
`variant` | `"primary" \| "danger" \| "success" \| "warning" \| "info" \| undefined` | "primary" | 
`size` | `"md" \| "sm" \| undefined` | "md" | Track thickness.
`ui` | `Partial<{ root: UiPartValue; track: UiPartValue; fill: UiPartValue; }> \| undefined` |  | 

## Slots

_None._

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`fillEl` | `HTMLElement \| null` | 

