# useDialog

The headless engine behind `<Dialog>`.

The headless modal behaviour behind `<Dialog>`: a cancelable close, Escape through the shared layer stack, a Tab focus trap, scroll lock, initial focus and focus return, and optional back-button dismiss. It doesn't render an overlay or listen for outside clicks: call `requestClose('outside', event)` from your own overlay. `<Dialog>` adds the teleport, overlay, panel, title ids and animations. `<Drawer>` and `<BottomSheet>` are built on `<Dialog>`.

```ts
import { useDialog } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { shallowRef, useTemplateRef } from 'vue'
import { useDialog } from 'vael-ui'

const open = shallowRef(false)
const panelEl = useTemplateRef<HTMLElement>('panel')
const wrapperEl = useTemplateRef<HTMLElement>('wrapper')
const { close, requestClose } = useDialog(open, { panelEl, wrapperEl })
</script>

<template>
  <button @click="open = true">Edit profile</button>
  <Teleport to="body">
    <div v-if="open" ref="wrapper" class="wrapper">
      <div class="overlay" @click="requestClose('outside', $event)" />
      <div ref="panel" role="dialog" aria-modal="true" aria-label="Edit profile" tabindex="-1">
        <input placeholder="Name" />
        <button @click="close">Save</button>
      </div>
    </div>
  </Teleport>
</template>
```

## Parameters

Name | Type | Description
--- | --- | ---
`open` | `Ref<boolean>` | First argument. You set it to `true`; the dialog sets it to `false` when it closes.
`panelEl` | `Ref<HTMLElement \| null>` | The dialog panel. Focus is trapped inside it, and it takes focus itself when it holds nothing focusable.
`wrapperEl` | `Ref<HTMLElement \| null>` | The outermost teleported node, holding the overlay and the panel. The layer stack and `inert` treat it as the dialog, so the overlay stays clickable. Falls back to `panelEl`.
`container` | `MaybeRefOrGetter<HTMLElement \| string \| null>` | Scopes the dialog to one element or selector: the scroll lock, modality and `inert` apply only inside it, and the rest of the page stays usable. A static container gets `position: relative`. Omit it for a page-level dialog.
`scrollTarget` | `MaybeRefOrGetter<HTMLElement \| string \| null>` | The element whose scrolling locks while open. Defaults to `container`, then `document.body`.
`closeOnEsc` | `MaybeRefOrGetter<boolean>` | Default `true`. Escape closes the topmost layer only.
`onOpenChange` | `(value: boolean, details: DialogOpenChangeDetails) => void` | Fires before every close, with `value: false`. `details.reason` is `'trigger'`, `'escape'`, `'outside'`, `'history'` or `'programmatic'`. Call `details.cancel()` to keep it open. It doesn't fire on open.
`beforeClose` | `() => ((done: () => void) => void) \| undefined` | A getter for a close hook. When it returns a function, the close waits until you call `done`. `isClosing` is `true` meanwhile.
`initialFocus` | `() => HTMLElement \| null \| undefined` | The element to focus on open. Return nothing to fall back to the panel's first focusable element, then the panel.
`modal` | `MaybeRefOrGetter<boolean>` | Default `true`. `false` skips the scroll lock, the Tab trap and the focus moves; Escape and layer stacking still work. Read once per open.
`closeOnHistoryBack` | `MaybeRefOrGetter<boolean>` | Default `false`. Pushes a history entry on open, so the back button or back gesture closes the dialog with the `'history'` reason instead of leaving the page. Any other close removes the entry again.
`holdScrollUntilExit` | `boolean` | Default `false`. Keeps the scroll lock after the close until you call `releaseScroll()`, so a returning scrollbar doesn't shift the panel during its exit. It releases on its own after 1000 ms.

## Returns

Name | Type | Description
--- | --- | ---
`isClosing` | `Ref<boolean>` | `true` while a `beforeClose` hook holds the close.
`close` | `() => void` | Closes with the `'programmatic'` reason, through `onOpenChange` and `beforeClose`.
`requestClose` | `(reason: DialogCloseReason, event?: Event) => void` | Closes with your own reason: `'outside'` from an overlay click, `'trigger'` from a close button.
`cancelClose` | `() => void` | Drops a close that a `beforeClose` hook holds.
`container / scrollTarget` | `Ref<HTMLElement \| null>` | The resolved elements, or `null`. Teleport into `container` when it is set.
`contained` | `ComputedRef<boolean>` | True when `container` resolved to an element. Position the dialog `absolute` instead of `fixed` then.
`layerIndex` | `() => number` | This dialog's position in the shared layer stack, or `-1` while closed. Add it to your z-index so a dialog opened later stacks on top.
`releaseScroll` | `() => void` | Ends the lock that `holdScrollUntilExit` keeps. Call it when your exit animation ends, such as a Transition's `@after-leave`. Does nothing while open.
