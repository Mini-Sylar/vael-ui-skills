# ButtonGroup

Joins Buttons into one segmented control. With a `v-model` and a `value` on each Button, they become toggles: one on at a time, or several with `multiple`.

```ts
import { ButtonGroup } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'horizontal'` | Whether the buttons sit in a row or a column.
`ariaLabel` | `string \| undefined` |  | Accessible name for the group.
`multiple` | `boolean \| undefined` | `false` | With `v-model`: several buttons can be on at once, and the model is an array of their `value`s. Otherwise one at a time.
`allowEmpty` | `boolean \| undefined` | `true` | With `v-model`, one at a time: clicking the button that's on turns it off. Set `false` for a choice that always has one (text alignment).
`motion` | `ButtonGroupMotion \| undefined` |  | How it moves: its appearance (`{ initial: { root: { opacity: 0, y: 4 } } }`); its buttons keep their own `motion`. `false` turns its own animation off.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `ButtonGroupValue \| readonly ButtonGroupValue[] \| null \| undefined` |  | Opt-in toggles. Bind it, and every Button inside with a `value` turns on and off, with `aria-pressed` set for you: one value at a time, or an array with `multiple`. Unbound, the group is purely visual, as before.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | The grouped buttons.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: ButtonGroupValue \| readonly ButtonGroupValue[] \| null \| undefined]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `ButtonGroupMotion` (exported from `'vael-ui'`); `false` turns it off. ButtonGroup: its appearance; its buttons keep their own `motion`.

- Parts: `root`
- Keys: `spring`, `duration`, `easing`, `initial`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

