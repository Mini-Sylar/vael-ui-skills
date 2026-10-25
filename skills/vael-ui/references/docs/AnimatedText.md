# AnimatedText

Swaps one text for the next with a short fade and lift, easing the width so nothing jumps.

```ts
import { AnimatedText } from 'vael-ui' // or 'vael-ui/vapor'
```

```vue
<Button @click="copy"><AnimatedText :value="copied ? 'Copied' : 'Copy link'" /></Button>
```

The new text rises 4px into place as the old one fades a third faster (150ms, no blur), and the box eases to the new width, so neighbors slide instead of jumping. With `motion="false"` it swaps at once and the exposed `el` is yours to animate (GSAP etc.). Select, CascadeSelect and TreeSelect use it for the selected label; `Input` uses it for `animated`. Tune with `--ui-duration-text` (150ms) and `--ui-ease-text`; reduced motion drops the lift.

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`value` | `string` |  | The text to show. Changing it crossfades to the new text.
`motion` | `AnimatedTextMotion \| undefined` | `true` | How it moves: its appearance (`{ initial: { root: { opacity: 0, y: 4 } } }`). `false` swaps the text at once. Unset, it follows the nearest `ConfigProvider`.
`split` | `"words" \| "letters" \| undefined` |  | A new value comes in a word or a letter at a time, each after the one before. Screen readers still read it whole. Animate the pieces yourself with `motion: { initial: { piece: … } }`.
`ui` | `Partial<{ root: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

_None._

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `AnimatedTextMotion` (exported from `'vael-ui'`); `false` turns it off. AnimatedText: its appearance and the swap's timing.

- Parts: `root`, `piece`
- Keys: `spring`, `duration`, `easing`, `initial`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.
