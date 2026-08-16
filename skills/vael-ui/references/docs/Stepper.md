# Stepper

```ts
import { Stepper } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | 
`orientation` | `"horizontal" \| "vertical" \| undefined` | "horizontal" | 
`linear` | `boolean \| undefined` | true | Steps ahead of the active one are only clickable once reached (no skipping ahead). Past/current steps stay clickable.
`clickable` | `boolean \| undefined` | true | `false` renders a pure display/progress indicator — no click handling at all.
`ui` | `Partial<{ root: UiPartValue; step: UiPartValue; trigger: UiPartValue; circle: UiPartValue; content: UiPartValue; label: UiPartValue; description: UiPartValue; connector: UiPartValue; }> \| undefined` |  | 
`modelValue` | `number \| undefined` | 0 | 

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; index: number; active: boolean; completed: boolean; disabled: boolean; }` | 

## Events

Name | Type | Description
--- | --- | ---
`change` | `[index: number, item: T]` | 
`update:modelValue` | `[value: number]` | 

## Exposed

_None._

