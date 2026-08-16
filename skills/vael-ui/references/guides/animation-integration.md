# Bring your own animation

Every component that animates ships a plain CSS transition, but exposes a real escape hatch to drive it with GSAP, motion-v, or nothing at all. Check a component's own [props/slots/exposed reference](../docs/_INDEX.md) for which of these it has; not every component has all three.

## `motionCss`

`false` disables the built-in CSS transition. The component still changes state instantly, leaving the DOM ready for you to animate yourself.

## `forceMount`

Keeps content mounted while closed instead of removing it on `v-if`. Pair with `<AnimatePresence>` and the default slot's `isClosing` flag to own exit timing yourself.

## `beforeClose(done)`

For imperative libraries: pass a function that receives `done`. The component stays mounted and `isClosing` stays `true` until you call it.

## Example: motion-v, Popover

`forceMount` plus the default slot's `isClosing` flag, wrapped in `<AnimatePresence>`:

```vue
<script setup lang="ts">
import { AnimatePresence, motion } from 'motion-v'
import { Popover } from 'vael-ui'
</script>

<template>
  <Popover force-mount v-model:open="open">
    <template #trigger="{ setTriggerEl }">
      <button :ref="setTriggerEl">Open</button>
    </template>
    <template #default="{ isClosing }">
      <AnimatePresence>
        <motion.div
          v-if="!isClosing"
          :initial="{ opacity: 0, scale: 0.95 }"
          :animate="{ opacity: 1, scale: 1 }"
          :exit="{ opacity: 0, scale: 0.95 }"
        >
          Popover content
        </motion.div>
      </AnimatePresence>
    </template>
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
