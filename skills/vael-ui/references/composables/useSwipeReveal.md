# useSwipeReveal

The headless engine behind `<SwipeToReveal>`.

The drag engine behind `<SwipeToReveal>`. You drag a row's content sideways to uncover an actions panel on its leading or trailing edge. A press must move 8 px before it counts as a drag, and a mostly vertical move hands the gesture back to the list's scroll. On release, the row opens or closes based on how fast and how far you dragged. The component adds the action panels, their measured widths, focus handling and the `change` event.

```ts
import { useSwipeReveal } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { ref } from 'vue'
import { useSwipeReveal } from 'vael-ui'

const open = ref(false)
const { offset, onContentPointerdown, onContentClick } = useSwipeReveal(open, {
  leadingWidth: 0,
  trailingWidth: 96,
})
</script>

<template>
  <div class="row">
    <div class="row-actions"><button>Delete</button></div>
    <div
      class="row-content"
      :style="{ transform: `translateX(${offset}px)` }"
      @pointerdown="onContentPointerdown"
      @click.capture="onContentClick"
    >
      Invoice #1042
    </div>
  </div>
</template>

<style>
.row { position: relative; overflow: hidden; }
.row-actions { position: absolute; inset-block: 0; inset-inline-end: 0; inline-size: 96px; }
/* pan-y keeps vertical list scrolling native while the row takes horizontal drags. */
.row-content { position: relative; touch-action: pan-y; }
</style>
```

## Parameters

Name | Type | Description
--- | --- | ---
`open` | `Ref<boolean>` | First argument. Written on every settled release, `reveal()` and `close()`. Setting it yourself opens or closes the row at its default side.
`leading` | `MaybeRefOrGetter<boolean>` | Default `false`. Whether a leading actions panel exists to reveal.
`trailing` | `MaybeRefOrGetter<boolean>` | Default `true`. Whether a trailing actions panel exists to reveal.
`leadingWidth` | `MaybeRefOrGetter<number>` | Required. The leading panel's width in px, its fully open position. A getter that reads the panel's `offsetWidth` keeps it measured.
`trailingWidth` | `MaybeRefOrGetter<number>` | Required. The trailing panel's width in px.
`disabled` | `MaybeRefOrGetter<boolean>` | Default `false`. Ignores new presses while `true`.
`onCommit` | `(openSide: 'leading' \| 'trailing' \| null) => void` | Fires once per settled interaction with the side left open, or `null` when the row closed. `reveal()` and `close()` fire it too.

## Returns

Name | Type | Description
--- | --- | ---
`offset` | `Ref<number>` | The live signed offset in px for `translateX()`: positive shows the leading panel, negative the trailing one, `0` is closed. Past either end it stretches with rubber-band resistance.
`openSide` | `Ref<'leading' \| 'trailing' \| null>` | The side that is open or settling open, or `null` when closed.
`onContentPointerdown` | `(event: PointerEvent) => void` | Bind it on the content. Only the primary button starts tracking.
`onContentClick` | `(event: MouseEvent) => void` | Bind it as `@click.capture` on the content. It swallows the click that follows a drag, and a tap on an open row closes it instead of reaching the row's own click.
`reveal` | `(side?: 'leading' \| 'trailing') => void` | Opens one side. With a single panel you can leave out `side`; with both it defaults to `trailing`. Does nothing for a side that has no panel.
`close` | `() => void` | Closes the row.
`isDragging` | `Ref<boolean>` | `true` once a press passes the 8 px drag threshold, until release. A plain tap never sets it.
