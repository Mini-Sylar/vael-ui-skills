# Rating

Click, drag, or use arrow keys to pick a value on a row of stars, with optional half-step precision.

```ts
import { Rating } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`max` | `number \| undefined` | `5` | Number of stars, which is also the highest rating.
`allowHalf` | `boolean \| undefined` | `false` | Half-star precision, for both pointer and keyboard (arrow keys step by `0.5`).
`readonly` | `boolean \| undefined` | `false` | Shows the rating but ignores pointer and keyboard input. Stays focusable.
`disabled` | `boolean \| undefined` | `false` | Disables the rating and blocks interaction.
`size` | `"sm" \| "md" \| "lg" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | Native `name` for form submission, via a hidden input.
`valueText` | `((value: number) => string) \| undefined` |  | Drives `aria-valuetext`. Defaults to the `rating.valueText` message ("{value} of {max}").
`motion` | `RatingMotion \| undefined` | `true` | How it moves: its appearance, star by star (`{ initial: { item: { opacity: 0, scale: 0.6 } }, stagger: 0.04 }`), and its states (`{ states: { disabled: { root: { opacity: 0.5 } } } }`). `false` turns its own animation off, the built-in fill sweep and commit pop too.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `number \| undefined` | `0` | Current rating; `0` means unrated.

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: number]` | Fires when `modelValue` changes (`v-model`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`itemEls` | `HTMLElement[] \| null` | Star elements, one per star.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `RatingMotion` (exported from `'vael-ui'`); `false` turns it off. Rating: its appearance, star by star, and states.

- Parts: `root`, `item`
- States: `disabled`, `focus`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `stagger`

See the [Motion guide](../guides/motion.md) for what each key does.

