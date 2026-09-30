# Slider

Drag a handle (or two, for a range) along a track to pick a numeric value.

```ts
import { Slider } from 'vael-ui' // or 'vael-ui/vapor'
```

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
`modelValue` | `number \| [number, number] \| undefined` | `0` | Current value. Bind a `[start, end]` tuple for a two-thumb range.

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: number \| [number, number]]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`trackEl` | `HTMLElement \| null` | Track element.
`fillEl` | `HTMLElement \| null` | Filled portion of the track.
`thumbEls` | `HTMLElement[] \| null` | Thumb elements, one per value (two in range mode). 

