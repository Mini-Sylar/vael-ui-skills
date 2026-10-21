# useMenu

The headless engine behind `<Menu>`.

The keyboard engine behind `<Menu>`, `<MenuList>` and the SpeedDial actions: roving focus, wrapping arrow keys, Home and End, and typeahead. It finds items by `role="menuitem"`, `menuitemcheckbox`, `menuitemradio` or a `data-menu-item` attribute, and skips disabled ones. It also listens for clicks on the list, so you don't bind `@click` on each item. Enter and Space click the focused item, so its own click handlers run once. The composable never closes anything: `onSelect` tells you an item fired, and you decide what happens next.

```ts
import { useMenu } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { onMounted, useTemplateRef } from 'vue'
import { useMenu } from 'vael-ui'

const listEl = useTemplateRef<HTMLElement>('list')
const { onKeydown, initRoving } = useMenu({
  listEl,
  onSelect: (itemEl) => console.log('run', itemEl.dataset.action),
})
// An always-visible list needs one Tab stop without taking focus on mount.
onMounted(initRoving)
</script>

<template>
  <div ref="list" role="menu" aria-label="File" @keydown="onKeydown">
    <button role="menuitem" data-action="copy">Copy</button>
    <button role="menuitem" data-action="rename">Rename</button>
    <button role="menuitem" data-action="delete" disabled>Delete</button>
  </div>
</template>
```

## Parameters

Name | Type | Description
--- | --- | ---
`listEl` | `Ref<HTMLElement \| null>` | The menu. Its matching descendants are the items.
`onSelect` | `(itemEl: HTMLElement) => void` | Fires when an enabled item is activated, by click or by Enter or Space while it has focus. It doesn't fire for submenu triggers. `<Menu>` closes here unless the item carries `data-keep-open`.
`onExpand` | `(itemEl: HTMLElement) => void` | Fires instead of `onSelect` when the activated item has `aria-haspopup="menu"`. ArrowRight on such an item clicks it, so it lands here too. `<Menu>` opens the nested submenu.
`onCollapse` | `() => void` | Fires on ArrowLeft. Wire it only when this list is a submenu, to close it and return focus to the parent row. Without it, ArrowLeft does nothing.

## Returns

Name | Type | Description
--- | --- | ---
`onKeydown` | `(event: KeyboardEvent) => void` | Bind it on the list. Up and Down wrap around, Home and End jump to the ends, and printable keys run a typeahead over each item's text. The typeahead buffer resets after 500 ms.
`focusFirst` | `() => void` | Gives the first enabled item `tabindex="0"` and focuses it. Call it when a popup menu opens.
`focusItem` | `(el: HTMLElement \| undefined) => void` | Makes `el` the Tab stop and focuses it, such as a submenu's parent row after the submenu closes.
`initRoving` | `() => void` | Gives the first enabled item `tabindex="0"` without focusing it. Call it once on mount for a list that is always on screen, like `<MenuList>`.
