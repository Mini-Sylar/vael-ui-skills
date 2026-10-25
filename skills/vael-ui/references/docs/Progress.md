# Progress

A bar or ring showing completion of a determinate or indeterminate task.

```ts
import { Progress } from 'vael-ui' // or 'vael-ui/vapor'
```

A ring's diameter is the `--ui-progress-ring-size` custom property on the root (`3rem`, or `1.5rem` at `size="sm"`). A gauge keeps that circle's scale and crops to its arc.

**Stacked segments.** Pass an array as `value`: each entry is `{ value: number; label?: string; variant?: ProgressVariant; color?: string }` (`import type { ProgressSegment } from 'vael-ui'`), in the same units as `max`. Segments stack in order and shade from the bar's `variant` unless they set their own. The legend appears once any segment has a `label`; each row shows a color swatch, the label and its percent of `max`. To show your own figures (GB, hours), put them in `label` (`'Photos · 40 GB'`) and keep the built-in row. `#legend` replaces a row's contents, swatch included, and gets `{ segment, index, percent }` but not the shaded color, so give each segment a `color` if you draw your own swatch. A ring with segments is a donut. Size a ring with a class: `.gauge { --ui-progress-ring-size: 8rem }`.

```vue
<Progress :value="[{ value: 40, label: 'Photos' }, { value: 25, label: 'Video' }]" :max="100" />
<Progress type="circle" :sweep="180" :value="72" v-slot="{ percent }">{{ Math.round(percent) }}%</Progress>
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`value` | `number \| ProgressSegment[] \| null \| undefined` |  | One value, or segments that stack into one bar (a storage breakdown, a budget). `null`/`undefined` renders the indeterminate (looping) state.
`max` | `number \| undefined` | `100` | Value that counts as complete; Progress clamps `value` to `0`–`max`.
`label` | `string \| undefined` |  | Accessible label for the progress bar.
`valueText` | `string \| undefined` |  | Human-readable reading for assistive tech (`aria-valuetext`), e.g. "42 GB of 100 GB".
`variant` | `'primary' \| 'danger' \| 'success' \| 'warning' \| 'info' \| undefined` | `'primary'` | Fill color variant for one value, and the base color segments shade from.
`type` | `"circle" \| "line" \| undefined` | `'line'` | `'circle'` draws a ring; the default slot sits in its middle. Segments work in both.
`sweep` | `number \| undefined` | `360` | How much of the circle a ring covers, in degrees, with the gap centred at the bottom: `180` is a half-ring gauge, `270` a speedometer. The value always fills that whole arc.
`size` | `"sm" \| "md" \| undefined` | `'md'` | Track thickness, or the ring's size and thickness.
`legend` | `boolean \| undefined` | `true` | Renders a legend under stacked segments that have labels.
`motion` | `ProgressMotion \| undefined` | `true` | How it moves: its appearance (`{ initial: { track: { opacity: 0 } } }`) and its states (`{ states: { complete: { root: { scale: [1, 1.02, 1] } } } }`). `{ when: 'visible' }` fills it once it scrolls into view. `false` skips the fill transition; drive `fillEl`/`fillEls` yourself.
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
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `ProgressMotion` (exported from `'vael-ui'`); `false` turns it off. Progress: its appearance, states, and the fill's timing and `when: 'visible'`.

- Parts: `root`, `track`, `fill`, `legend`
- States: `complete`, `indeterminate`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `when`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.
