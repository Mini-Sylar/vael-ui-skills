# usePullToRefresh

The headless engine behind `<PullToRefresh>`.

The gesture engine and state machine behind `<PullToRefresh>`. A downward drag at scroll top pulls a distance you render, then a release past `threshold` runs `onRefresh`. It moves through `idle`, `pulling`, `ready`, `loading` and `done`, then back to `idle`. Touch drives it through a non-passive `touchmove`, the one way to hold back the browser's own scroll and bounce. A mouse drags it through pointer events. `<PullToRefresh>` adds the pull zone, the arrow and loader indicator and the `#indicator` slot.

```ts
import { usePullToRefresh } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { useTemplateRef } from 'vue'
import { usePullToRefresh } from 'vael-ui'

const scrollEl = useTemplateRef<HTMLElement>('scroller')
const { state, pullDistance } = usePullToRefresh({
  scrollEl,
  onRefresh: () => fetchInbox(),
})
</script>

<template>
  <div ref="scroller" class="inbox">
    <div class="pull-zone" :style="{ blockSize: `${pullDistance}px` }">
      {{ state === 'loading' ? 'Refreshing' : state === 'ready' ? 'Release to refresh' : '' }}
    </div>
    <ul>...</ul>
  </div>
</template>
```

## Parameters

Name | Type | Description
--- | --- | ---
`scrollEl` | `Ref<HTMLElement \| null>` | The scroll container. A pull starts only when it rests at `scrollTop` 0 and the drag moves down at least 8 px. Scrolling away mid drag hands the gesture back.
`onRefresh` | `() => Promise<void> \| void` | Required. Awaited while `state` is `loading`. A rejection still settles back to `idle`, and the error is swallowed, so handle it inside.
`threshold` | `MaybeRefOrGetter<number \| undefined>` | Default `60`. The pull distance in px at which a release commits. A quick flick past half of it commits too.
`maxPull` | `MaybeRefOrGetter<number \| undefined>` | Default `80`. Past this many px, the pull meets rubber-band resistance. The zone holds at this height while loading.
`doneHoldMs` | `MaybeRefOrGetter<number \| undefined>` | Default `650`. How long `done` shows, in ms, before returning to `idle`.

## Returns

Name | Type | Description
--- | --- | ---
`state` | `Ref<'idle' \| 'pulling' \| 'ready' \| 'loading' \| 'done'>` | `ready` means a release now refreshes.
`pullDistance` | `Ref<number>` | The current pull in px, dampened past `maxPull`. Bind it to the zone height.
`progress` | `ComputedRef<number>` | From 0 to 1, relative to `threshold`, not `maxPull`. It reaches 1 exactly where `ready` starts.
`refresh` | `() => Promise<void>` | Runs the same `loading` and `done` sequence a release does, from code. Does nothing while a refresh is already running or showing `done`.
