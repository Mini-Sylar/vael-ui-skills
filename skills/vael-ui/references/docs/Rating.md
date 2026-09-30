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
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Control size.
`name` | `string \| undefined` |  | Native `name` for form submission, via a hidden input.
`valueText` | `((value: number) => string) \| undefined` |  | Drives `aria-valuetext`. Defaults to the `rating.valueText` message ("{value} of {max}").
`motionCss` | `boolean \| undefined` | `true` | `false` skips the built-in fill-sweep and commit-pop animations.
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

