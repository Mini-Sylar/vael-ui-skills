# springEasing

Turns a spring into a CSS `linear()` curve and the time it takes to settle, in ms. It's the curve vael-ui uses for `ConfigProvider`'s `:motion="{ spring }"`. The result has the same shape as the options `element.animate()` takes, so your own animations can move on the same spring without a library. Damping below 1 overshoots.

```ts
import { springEasing } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```ts
import { springEasing } from 'vael-ui'

// { easing: 'linear(0, …, 1)', duration: 642 }
const spring = springEasing({ damping: 0.7, response: 0.4 })

panel.animate([{ opacity: 0, scale: 0.96 }, { opacity: 1, scale: 1 }], spring)

// Or in CSS
el.style.transition = `transform ${spring.duration}ms ${spring.easing}`
```

## Parameters

Name | Type | Description
--- | --- | ---
`spring` | `{ damping?: number; response?: number }` | Damping 1 settles without overshoot; below 1 it bounces. Response is roughly the time to arrive, in seconds.

## Returns

Name | Type | Description
--- | --- | ---
`easing` | `string` | A CSS `linear()` easing that follows the spring.
`duration` | `number` | How long the spring takes to settle, in ms.
