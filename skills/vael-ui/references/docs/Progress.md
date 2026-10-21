# Progress

A bar or ring showing completion of a determinate or indeterminate task.

```ts
import { Progress } from 'vael-ui' // or 'vael-ui/vapor'
```

A ring's diameter is the `--ui-progress-ring-size` custom property on the root (`3rem`, or `1.5rem` at `size="sm"`). A gauge keeps that circle's scale and crops to its arc.

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`value` | `number \| ProgressSegment[] \| null \| undefined` |  | One value, or segments that stack into one bar (a storage breakdown, a budget). `null`/`undefined` renders the indeterminate (looping) state.
`max` | `number \| undefined` | `100` | Value that counts as complete; Progress clamps `value` to `0`–`max`.
`label` | `string \| undefined` |  | Accessible label for the progress bar.
`valueText` | `string \| undefined` |  | Human-readable reading for assistive tech (`aria-valuetext`), e.g. "42 GB of 100 GB".
`variant` | `ProgressVariant \| undefined` | `'primary'` | Fill color variant for one value, and the base color segments shade from.
`type` | `"circle" \| "line" \| undefined` | `'line'` | `'circle'` draws a ring; the default slot sits in its middle. Segments work in both.
`sweep` | `number \| undefined` | `360` | How much of the circle a ring covers, in degrees, with the gap centred at the bottom: `180` is a half-ring gauge, `270` a speedometer. The value always fills that whole arc.
`size` | `"sm" \| "md" \| undefined` | `'md'` | Track thickness, or the ring's size and thickness.
`legend` | `boolean \| undefined` | `true` | Renders a legend under stacked segments that have labels.
`motionCss` | `boolean \| undefined` | `true` | `false` skips the fill transition; drive `fillEl`/`fillEls` yourself.
`ui` | `Partial<{ root: UiPartValue; track: UiPartValue; fill: UiPartValue; legend: UiPartValue; legendItem: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{ percent: number; value: number; }` | Content in the middle of a ring, or inside the arch of a gauge, such as the percentage.
`legend` | `{ segment: ProgressSegment; index: number; percent: number; }` | Replaces one legend row.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`fillEl` | `HTMLElement \| SVGPathElement \| null` | The first fill: a bar's `<div>` or a ring's `<path>`. Its share comes from the `--ui-progress-scale` custom property, and a ring segment's start from `--ui-progress-start`.
`fillEls` | `(HTMLElement \| SVGPathElement)[]` | Every fill, one per stacked segment, in order.
