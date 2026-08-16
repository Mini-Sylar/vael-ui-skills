# PullToRefresh

A pull-down gesture at the top of a scroll container that triggers a refresh.

```ts
import { PullToRefresh } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`onRefresh` | `() => void \| Promise<void>` |  | 
`threshold` | `number \| undefined` |  | 
`maxPull` | `number \| undefined` |  | 
`scrollEl` | `HTMLElement \| { el: HTMLElement \| null; } \| null \| undefined` |  | Detects gestures on this element instead of the root. Defaults to the root.
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

