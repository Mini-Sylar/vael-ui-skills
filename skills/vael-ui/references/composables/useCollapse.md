# useCollapse

The headless engine behind `<Collapsible>`.

The height animation behind `<Collapsible>` and `<AccordionItem>`. It animates a panel between `0px` and its content height through inline `block-size`, and the timing comes from your CSS transition. Once open and settled, the inline style is removed, so the panel follows later content changes. Content that grows while the panel opens retargets the animation instead of being clipped. Use it to collapse any element of your own; `<Collapsible>` adds the trigger, ARIA wiring and `data-state`.

```ts
import { useCollapse } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { shallowRef, useTemplateRef } from 'vue'
import { useCollapse } from 'vael-ui'

const open = shallowRef(false)
const panelEl = useTemplateRef<HTMLElement>('panel')
const { style, state } = useCollapse(open, { el: panelEl })
</script>

<template>
  <button :aria-expanded="open" @click="open = !open">Details</button>
  <div ref="panel" class="panel" :style="style" :data-state="state">
    <p>Shipping takes 3 to 5 days.</p>
  </div>
</template>

<style scoped>
.panel { transition: block-size 200ms ease-out; }
</style>
```

## Parameters

Name | Type | Description
--- | --- | ---
`open` | `Ref<boolean>` | First argument. Every change starts a transition from the panel's current height.
`el` | `Ref<HTMLElement \| null>` | The panel. It needs a `block-size` transition in your CSS. Without one, it opens and closes at once.
`motion` | `() => boolean` | Return `false` to turn off the built-in animation. By default that hands the height over to you: no inline styles at all, open or closed, so your own tween is never cut off. `state` still reports `open` and `closed`.
`handOff` | `() => boolean` | Only matters while `motion` returns `false`. Default `true` hands the height over. Return `false` to keep the collapsed styles and make the change instant instead, for an app-wide "no animations" setting.

## Returns

Name | Type | Description
--- | --- | ---
`style` | `Ref<Record<string, string>>` | Bind it on the panel. Closed: `block-size: 0px`, `overflow: hidden`, `visibility: hidden`. Moving: a pinned `block-size` in px. Open and settled: empty.
`state` | `Ref<'open' \| 'closed' \| 'opening' \| 'closing'>` | Where the panel is. It settles on `transitionend` for `block-size`, or after the longest transition duration plus delay and a 50 ms buffer, whichever comes first.
