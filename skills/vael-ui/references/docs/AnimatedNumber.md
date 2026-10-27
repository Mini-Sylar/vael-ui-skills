# AnimatedNumber

Shows a number that rolls each changed digit to its new value, formatted for any locale.

```ts
import { AnimatedNumber } from 'vael-ui' // or 'vael-ui/vapor'
```

```vue
<AnimatedNumber :value="revenue" :format="{ style: 'currency', currency: 'USD' }" />
```

Only changed digits roll; a digit that appears or disappears fades while the number makes room. Screen readers get the formatted text, not the reels. Right-to-left locales keep the browser's own order. Tune the roll with `--ui-duration-number` (500ms) and `--ui-ease-number`; `ConfigProvider`'s `motion="false"` zeroes it too.

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`value` | `number` |  | The number to show. Changing it rolls each changed digit to its new value.
`format` | `Intl.NumberFormatOptions \| undefined` |  | `Intl.NumberFormat` options: currency, percent, compact, fraction digits and so on.
`locale` | `string \| undefined` |  | BCP 47 locale for the digits and separators. Unset, it follows the nearest `ConfigProvider`, then the browser.
`motion` | `AnimatedNumberMotion \| undefined` | `true` | How it moves: its appearance (`{ initial: { root: { opacity: 0, y: 4 } } }`). `{ when: 'visible' }` holds every digit at 0 until the number scrolls into view, then rolls them to the value. `false` swaps digits without rolling. Unset, it follows the nearest `ConfigProvider`.
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

`motion` takes `AnimatedNumberMotion` (exported from `'vael-ui'`); `false` turns it off. AnimatedNumber: its appearance, timing for the roll, and `when: 'visible'`.

- Parts: `root`
- Keys: `spring`, `duration`, `easing`, `initial`, `when`, `replay`, `appear`, `onAfterEnter`

See the [Motion guide](../guides/motion.md) for what each key does.
