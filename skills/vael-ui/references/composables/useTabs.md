# useTabs

The headless engine behind `<Tabs>`.

The keyboard engine behind `<Tabs>`: selection, roving focus and arrow key navigation from the APG tabs pattern. It renders nothing. You render the `role="tab"` elements and bind `onKeydown` on the list. `<Tabs>` adds the markup, ids, panel wiring and the sliding indicators on top. Use the composable when your tabs need markup the `#item` slot can't give you.

```ts
import { useTabs } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { shallowRef, useTemplateRef } from 'vue'
import { useTabs } from 'vael-ui'

const tabs = ['general', 'billing', 'team'] as const
const active = shallowRef<(typeof tabs)[number]>('general')
const listEl = useTemplateRef<HTMLElement>('list')
const { select, onKeydown } = useTabs(active, { items: tabs, listEl })
</script>

<template>
  <div ref="list" role="tablist" @keydown="onKeydown">
    <button
      v-for="tab in tabs"
      :key="tab"
      role="tab"
      :data-tab-value="tab"
      :aria-selected="active === tab"
      :tabindex="active === tab ? 0 : -1"
      @click="select(tab)"
    >
      {{ tab }}
    </button>
  </div>
</template>
```

## Parameters

Name | Type | Description
--- | --- | ---
`active` | `Ref<T>` | First argument. The selected item. `select()` and the arrow keys write to it.
`items` | `MaybeRefOrGetter<readonly T[]>` | Every tab, in order. Items should be strings or numbers: a tab element is matched by `data-tab-value="String(item)"`, then by its position among the `role="tab"` elements.
`listEl` | `Ref<HTMLElement \| null>` | The tab list. Its `role="tab"` descendants are the tabs. A tab with `disabled` or `aria-disabled="true"` is skipped.
`orientation` | `MaybeRefOrGetter<'horizontal' \| 'vertical'>` | Horizontal tabs move with Left and Right arrows, vertical tabs with Up and Down. Default `horizontal`.
`activation` | `MaybeRefOrGetter<'automatic' \| 'manual'>` | Default `automatic`: the arrow keys select as they move focus. `manual`: the arrow keys only move focus, and Enter or Space selects the focused tab. Use `manual` when switching tabs is expensive, such as a tab that loads data.
`onChange` | `(item: T) => void` | Fires when the selection changes, by click or keyboard.

## Returns

Name | Type | Description
--- | --- | ---
`select` | `(item: T) => void` | Selects `item` and calls `onChange`. Does nothing when `item` is already selected. Bind it to each tab's click.
`onKeydown` | `(event: KeyboardEvent) => void` | Bind it on the list. Arrow keys move to the next enabled tab and wrap around the ends. Home and End jump to the first and last enabled tab. Focus follows after the next render.
`focused` | `Ref<T>` | The tab that holds keyboard focus. It differs from `active` only in `manual` mode, where you give it `tabindex="0"` instead of the selected tab.
