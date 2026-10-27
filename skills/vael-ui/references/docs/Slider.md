# Slider

Drag a handle (or two, for a range) along a track to pick a numeric value.

```ts
import { Slider } from 'vael-ui' // or 'vael-ui/vapor'
```

## Notes

`aria-label`, `aria-labelledby` and `aria-describedby` go on the `role="slider"` thumb, not the wrapper; a surrounding `Field` label works too. In range mode each thumb is named after the label plus the `uiKit.slider.minimum` / `uiKit.slider.maximum` messages, e.g. "Price Minimum" and "Price Maximum".

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`min` | `number \| undefined` | `0` | Lowest allowed value.
`max` | `number \| undefined` | `100` | Highest allowed value.
`step` | `number \| undefined` | `1` | Increment the value snaps to; arrow keys move one step, Page Up/Down ten.
`orientation` | `"horizontal" \| "vertical" \| undefined` | `'horizontal'` | Lays the track out horizontally or vertically.
`disabled` | `boolean \| undefined` | `false` | Disables the slider and blocks interaction.
`invalid` | `boolean \| undefined` | `false` | Shows the invalid state. A surrounding `Field` in error sets it too.
`name` | `string \| undefined` |  | Native `name` for form submission, via one hidden input per thumb.
`valueText` | `((value: number) => string) \| undefined` |  | Drives `aria-valuetext`, e.g. `(v) => \`$${v}\`` for a currency slider.
`ui` | `Partial<{ root: UiPartValue; track: UiPartValue; fill: UiPartValue; thumb: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`motion` | `SliderMotion \| undefined` |  | How it moves: its appearance (`{ initial: { thumb: { opacity: 0, scale: 0.6 } } }`) and its states (`{ states: { focus: { thumb: { scale: '1.15' } } } }`). `{ when: 'visible' }` holds its appearance until it scrolls into view. `false` turns its own animation off.
`modelValue` | `number \| [number, number] \| undefined` | `0` | Current value. Bind a `[start, end]` tuple for a two-thumb range.

## Slots

Name | Type | Description
--- | --- | ---
`thumb` | `{ value: number; index: number; percent: number; active: boolean; dragging: boolean; }` | Content inside each thumb, such as a value bubble or an icon. The thumb keeps its role and keyboard handling.
`track` | `{ values: number[]; percents: number[]; dragging: boolean; }` | Content inside the track, under the thumbs, such as ticks or an inline label.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: number \| [number, number]]` | Fires when `modelValue` changes (`v-model`).
`change` | `[value: number \| [number, number]]` | Fires with the final value when you release a thumb or press a key, like a native range input's `change`. `v-model` updates on every move.

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`trackEl` | `HTMLElement \| null` | Track element.
`fillEl` | `HTMLElement \| null` | Filled portion of the track.
`thumbEls` | `HTMLElement[] \| null` | Thumb elements, one per value (two in range mode).
`dragging` | `boolean` | `true` while a thumb is being dragged.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `SliderMotion` (exported from `'vael-ui'`); `false` turns it off. Slider's `motion`: values or hooks for its parts (SliderPart), its states (`disabled`, `focus`), how its value changes.

- Parts: `root`, `track`, `fill`, `thumb`
- States: `disabled`, `focus`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `when`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.

