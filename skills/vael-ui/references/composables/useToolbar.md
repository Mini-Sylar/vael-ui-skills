# useToolbar

The headless engine behind `<Toolbar>`.

The engine behind `<Toolbar>`: one Tab stop for the whole toolbar, arrow keys between its controls (APG toolbar pattern), and Priority+ overflow. Controls are the list's buttons, links, inputs, selects, textareas and anything with a non-negative `tabindex`. Mark controls that may collapse with `data-toolbar-overflow`; when the toolbar runs out of room, they hide from the end backward and land in `collapsedItems`. `<Toolbar>` adds the groups, the hover highlight and a `…` Menu built from the collapsed controls' labels. Use the composable to give your own toolbar markup the same behaviour.

```ts
import { useToolbar } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { useTemplateRef } from 'vue'
import { useToolbar } from 'vael-ui'

const toolbarEl = useTemplateRef<HTMLElement>('toolbar')
const { onKeydown, collapsedItems, hasOverflow } = useToolbar(toolbarEl)
</script>

<template>
  <div ref="toolbar" role="toolbar" aria-label="Formatting" class="toolbar" @keydown="onKeydown">
    <button>Bold</button>
    <button>Italic</button>
    <button data-toolbar-overflow>Strikethrough</button>
    <button data-toolbar-overflow>Code</button>
    <button v-if="hasOverflow" data-toolbar-ellipsis aria-label="More">…</button>
  </div>
  <p>{{ collapsedItems.length }} hidden</p>
</template>

<style scoped>
.toolbar { display: flex; overflow: hidden; }
.toolbar > * { flex-shrink: 0; }
</style>
```

## Parameters

Name | Type | Description
--- | --- | ---
`listEl` | `Ref<HTMLElement \| null>` | First argument. The toolbar element. Overflow compares the sum of its direct children's sizes, plus its `gap`, with its own client size, so give the children `flex-shrink: 0`.
`orientation` | `MaybeRefOrGetter<'horizontal' \| 'vertical'>` | Horizontal toolbars use Left and Right arrows and measure width; vertical ones use Up and Down and measure height. Default `horizontal`.

## Returns

Name | Type | Description
--- | --- | ---
`onKeydown` | `(event: KeyboardEvent) => void` | Bind it on the toolbar. Arrow keys move to the next enabled control and wrap; Home and End jump to the ends. It ignores an event a child already handled with `preventDefault()`, so a text input's own arrow keys win.
`collapsedItems` | `Ref<HTMLElement[]>` | The `data-toolbar-overflow` controls hidden right now, in DOM order. A hidden control gets `data-toolbar-collapsed` and inline `visibility: hidden; position: absolute`, and leaves the arrow key order. Call `el.click()` from your overflow menu to run its handlers.
`hasOverflow` | `ComputedRef<boolean>` | True while anything is collapsed. Render your `…` trigger only then, and mark it `data-toolbar-ellipsis` so its size is reserved when controls come back.
`hasOverflowCandidates` | `ComputedRef<boolean>` | True whenever the toolbar has any `data-toolbar-overflow` control, collapsed or not. Use it to turn on `overflow: hidden`, so nothing spills out before the next measurement.
