# Rating

Click, drag, or use arrow keys to pick a value on a row of stars, with optional half-step precision.

```ts
import { Rating } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`max` | `number \| undefined` | 5 | 
`allowHalf` | `boolean \| undefined` | false | Half-star precision, for both pointer and keyboard (arrow keys step by 0.5).
`readonly` | `boolean \| undefined` | false | 
`disabled` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`name` | `string \| undefined` |  | Falls through to a hidden `<input>` → plain `<form>` participation.
`valueText` | `((value: number) => string) \| undefined` |  | Drives `aria-valuetext`. Defaults to the `rating.valueText` message ("{value} of {max}").
`motionCss` | `boolean \| undefined` | true | Gates the fill-sweep + commit-pop animation.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; }> \| undefined` |  | 
`modelValue` | `number \| undefined` | 0 | 

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: number]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`itemEls` | `HTMLElement[] \| null` | 

