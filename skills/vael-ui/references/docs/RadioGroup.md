# RadioGroup

Coordinates a set of Radio buttons so only one can be selected at a time.

```ts
import { RadioGroup } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`name` | `string \| undefined` |  | Native `name` shared by every Radio's input. Auto-generated when omitted.
`disabled` | `boolean \| undefined` | `false` | Disables every Radio in the group.
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'vertical'` | Lays the radios out in a row or a column.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `RadioGroupMotion \| undefined` |  | How it moves: its appearance (`{ initial: { root: { opacity: 0, y: 4 } } }`); each Radio keeps its own `motion`. `false` turns its own animation off.
`modelValue` | `string \| number \| null \| undefined` | `null` | Value of the selected Radio, or `null` when none is selected.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | The group's `Radio` items.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: string \| number \| null]` | Fires when `modelValue` changes (`v-model`).
`change` | `[value: string \| number \| null]` | Fires when you select a different Radio, with its value.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `RadioGroupMotion` (exported from `'vael-ui'`); `false` turns it off. RadioGroup: its appearance; its radios keep their own `motion`.

- Parts: `root`
- Keys: `spring`, `duration`, `easing`, `initial`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.

