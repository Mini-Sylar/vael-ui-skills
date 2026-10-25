# PullToRefresh

A pull-down gesture at the top of a scroll container that triggers a refresh.

```ts
import { PullToRefresh } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`onRefresh` | `() => void \| Promise<void>` |  | Runs when you release a pull past `threshold`. The spinner shows until its promise settles.
`threshold` | `number \| undefined` |  | How far you pull, in pixels, before the refresh triggers.
`maxPull` | `number \| undefined` |  | Maximum pull distance in pixels; the pull stops growing past it.
`scrollEl` | `HTMLElement \| { el: HTMLElement \| null; } \| null \| undefined` |  | Existing scroll container to detect pulls on. Unset, the root itself scrolls.
`motion` | `PullToRefreshMotion \| undefined` | `true` | How it moves: its appearance (`{ initial: { indicator: { opacity: 0 } } }`) and its states (`{ states: { refreshing: { indicator: { scale: '1.1' } } } }`). `false` skips the settle and indicator transitions; animate the `#indicator` slot or exposed `zoneEl` yourself. The live pull still tracks your finger.
`ui` | `Partial<{ root: UiPartValue; zone: UiPartValue; indicator: UiPartValue; bubble: UiPartValue; label: UiPartValue; arrow: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | Scrollable content.
`indicator` | `{ state: PullToRefreshState; progress: number; pullDistance: number; }` | Replaces the default arrow, spinner and label shown in the pull zone.

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`zoneEl` | `HTMLElement \| null` | The pull zone above the content; its height follows the pull.
`state` | `PullToRefreshState` | Current phase: `'idle'`, `'pulling'`, `'ready'`, `'loading'` or `'done'`.
`progress` | `number` | Pull distance as a fraction of `threshold`, from 0 to 1.
`refresh` | `() => Promise<void>` | Starts a refresh without a pull. No-op while one is already running.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `PullToRefreshMotion` (exported from `'vael-ui'`); `false` turns it off. PullToRefresh: its appearance and states.

- Parts: `root`, `indicator`
- States: `pulling`, `refreshing`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`

See the [Motion guide](../guides/motion.md) for what each key does.

