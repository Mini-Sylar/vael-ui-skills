# Stepper

```ts
import { Stepper } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Steps to show, each with a `label` and optional `description` and `disabled`.
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'horizontal'` | Lays the steps out in a row or a column.
`linear` | `boolean \| undefined` | `true` | Blocks clicks on steps you haven't reached yet, so you can't skip ahead. Every step up to the furthest one reached stays clickable, even after stepping back.
`clickable` | `boolean \| undefined` | `true` | `false` renders a display-only progress indicator with no click handling.
`motionCss` | `boolean \| undefined` | `true` | Built-in transitions: the check swap, circle colors and connector fill. `false` makes them instant.
`ui` | `Partial<{ root: UiPartValue; step: UiPartValue; trigger: UiPartValue; circle: UiPartValue; content: UiPartValue; label: UiPartValue; description: UiPartValue; connector: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `number \| undefined` | `0` | Index of the active step.

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; index: number; active: boolean; completed: boolean; disabled: boolean; }` | Custom label content for each step, replacing the default label and description.
`indicator` | `{ item: T; index: number; state: "active" \| "completed" \| "upcoming"; active: boolean; completed: boolean; disabled: boolean; }` | Replaces the content inside each step's circle (the number or check mark). The circle itself stays, so its size, border and focus ring still apply; restyle it via `ui.circle`.

## Events

Name | Type | Description
--- | --- | ---
`change` | `[index: number, item: T]` | Fires when you click a step and it becomes active.
`update:modelValue` | `[value: number]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`circleEls` | `HTMLElement[]` | Step circle elements, in order. A circle remounts when its step becomes clickable.
`connectorEls` | `HTMLElement[]` | Connector elements, one fewer than steps. Set `--ui-stepper-fill` (0 to 1) to drive a fill.
`labelEls` | `HTMLElement[]` | Step label elements, in order. Empty when the `item` slot replaces them.

