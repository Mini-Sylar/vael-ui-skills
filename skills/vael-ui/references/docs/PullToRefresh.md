# PullToRefresh

A pull-down gesture at the top of a scroll container that triggers a refresh.

```ts
import { PullToRefresh } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`onRefresh` | `() => void \| Promise<void>` |  | 
`threshold` | `number \| undefined` |  | How far to pull (in pixels) before the refresh triggers.
`maxPull` | `number \| undefined` |  | Maximum pull distance (in pixels) allowed before clamping.
`scrollEl` | `HTMLElement \| { el: HTMLElement \| null; } \| null \| undefined` |  | Detects gestures on this element instead of the root, for dropping into an existing scrollable layout as a thin wrapper. Defaults to the root, which then owns scrolling itself (`overflow-y: auto`); passing one leaves the root's own overflow untouched.
`ui` | `Partial<{ root: UiPartValue; zone: UiPartValue; indicator: UiPartValue; bubble: UiPartValue; label: UiPartValue; }> \| undefined` |  | 

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | 
`indicator` | `{ state: PullToRefreshState; progress: number; pullDistance: number; }` | 

## Events

_None._

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`state` | `PullToRefreshState` | 
`progress` | `number` | 
`refresh` | `() => Promise<void>` | 

