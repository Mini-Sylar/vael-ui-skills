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
`ui` | `Partial<{ root: UiPartValue; zone: UiPartValue; indicator: UiPartValue; bubble: UiPartValue; label: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.

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
`state` | `PullToRefreshState` | Current phase: `'idle'`, `'pulling'`, `'ready'`, `'loading'` or `'done'`.
`progress` | `number` | Pull distance as a fraction of `threshold`, from 0 to 1.
`refresh` | `() => Promise<void>` | Starts a refresh without a pull. No-op while one is already running. 

