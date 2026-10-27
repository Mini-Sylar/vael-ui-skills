# Stepper

Shows progress through a sequence of steps, and lets people move between them.

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
`motion` | `StepperMotion \| undefined` | `true` | How it moves: its parts' appearance, step by step (`{ initial: { step: { opacity: 0, y: 4 } }, stagger: 0.06 }`), over `root`, `step`, `circle`, `connector` and `content`. The built-in transitions are the check swap, circle colors and connector fill; `false` makes them instant.
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
`circleEls` | `HTMLElement[]` | Step circle elements, in order.
`connectorEls` | `HTMLElement[]` | Connector elements, one fewer than steps. Set `--ui-stepper-fill` (0 to 1) to drive a fill.
`labelEls` | `HTMLElement[]` | Step label elements, in order. Empty when the `item` slot replaces them.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `StepperMotion` (exported from `'vael-ui'`); `false` turns it off. Stepper: its parts' appearance and their states, step by step. Each step, circle and line plays its own: `current`, `completed` or `upcoming` (a line is `completed` once it's filled).

- Parts: `root`, `step`, `circle`, `connector`, `content`
- States: `current`, `completed`, `upcoming`, `disabled`, `hover`, `focus`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `onAfterEnter`, `stagger`

See the [Motion guide](../guides/motion.md) for what each key does.

