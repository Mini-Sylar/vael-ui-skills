# Bring your own animation

Every component that animates ships a plain CSS transition, and each one lets you take over: drive it with GSAP, motion-v, or nothing at all. Which hooks you get depends on what the component does.

## Which hooks does a component have?

- **Opens and closes** (Dialog, Popover, Tooltip, Menu, Select, DatePicker): `forceMount`, `beforeClose(done)` and an exposed `panelEl`. `forceMount` already turns the CSS transition off.
- **Changes in place** (Calendar, Checkbox, Radio, Collapsible, Pagination, PullToRefresh): `motionCss` turns the transition off, and exposed elements let you animate it yourself.
- **Items that come and go** (CommandPalette, Timeline, FileUpload): `motionCss` plus `@item-enter` and `@item-leave`.

A component's own [props/slots/exposed reference](../docs/_INDEX.md) lists exactly what it has.

## `motionCss`

`false` disables the built-in CSS transition. The component still changes state instantly, leaving the DOM ready for you to animate yourself.

On a `ConfigProvider`, `:motion-css="false"` means "no animation" for everything inside: panels snap open and closed, and toasts and list rows leave straight away, with no events to handle. Set it on the component itself when you want to take over its animation.

## `forceMount`

Keeps content mounted while closed instead of removing it on `v-if`, and turns off the built-in transition. Animate the exposed `panelEl` as it opens, and pair it with `beforeClose(done)` to run your own exit.

Don't wrap panel content in motion-v's `<AnimatePresence>` keyed on `isClosing`: inside a panel that hides with `v-show`, the enter doesn't play and the exit never reports back, so the close never finishes.

## `beforeClose(done)`

For imperative libraries: pass a function that receives `done`. The component stays mounted and `isClosing` stays `true` until you call it.

## Example: motion-v, Popover

`forceMount` hands you the panel. Animate `panelEl` in when it opens, and run the exit in `beforeClose(done)`, which keeps the panel up until you call `done()`:

```vue
<script setup lang="ts">
import { ref, useTemplateRef, watch } from 'vue'
import { animate } from 'motion-v'
import { Popover } from 'vael-ui'

const open = ref(false)
const popover = useTemplateRef('popover')

// Runs before the panel shows, so it starts hidden instead of flashing.
watch(open, (isOpen) => {
  const panel = popover.value?.panelEl
  if (!isOpen || !panel) return
  panel.style.opacity = '0'
  animate(panel, { opacity: [0, 1], scale: [0.95, 1] }, { duration: 0.2 })
})

// Popover keeps the panel up until done() is called.
async function beforeClose(done: () => void) {
  const panel = popover.value?.panelEl
  if (panel) await animate(panel, { opacity: 0, scale: 0.95 }, { duration: 0.15 })
  done()
}
</script>

<template>
  <Popover
    ref="popover"
    v-model:open="open"
    force-mount
    open-on-trigger-click
    :before-close="beforeClose"
  >
    <template #trigger>
      <button>Open</button>
    </template>
    <template #default>Popover content</template>
  </Popover>
</template>
```

## Example: GSAP, Dialog

`beforeClose(done)` plus the exposed `panelEl` ref, so the tween runs on the real panel element before it's removed:

```vue
<script setup lang="ts">
import { ref } from 'vue'
import gsap from 'gsap'
import { Dialog } from 'vael-ui'

const dialogRef = ref<InstanceType<typeof Dialog>>()
const open = ref(false)

function beforeClose(done: () => void) {
  gsap.to(dialogRef.value?.panelEl, {
    opacity: 0,
    y: 8,
    duration: 0.2,
    onComplete: done,
  })
}
</script>

<template>
  <Dialog ref="dialogRef" v-model:open="open" :before-close="beforeClose">
    Dialog content
  </Dialog>
</template>
```

## Known limitation: `<Transition>` + `<Teleport>` in Vue 3.6 Vapor before rc.4

If targeting Vapor specifically: Vue versions before `3.6.0-rc.4` had a bug where `<Transition>` wrapping a Teleported component (e.g. a force-mounted `Dialog`) didn't correctly defer DOM removal, so `AnimatePresence`-style exit animations never got to run through a Teleport boundary under Vapor. The imperative `beforeClose` pattern above was the only reliable option. Fixed in `3.6.0-rc.4` and later; pin at least that version if relying on the AnimatePresence pattern with Vapor.
