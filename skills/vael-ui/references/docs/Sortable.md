# Sortable

A drag-to-reorder list with a real keyboard path, not just a pointer one.

```ts
import { Sortable } from 'vael-ui' // or 'vael-ui/vapor'
```

**A board (cards moving between lists).** Share one `useSortableGroup()` handle across the lists via `:group`, give each a `group-id`, and move the item between your arrays in `onTransfer`; reorders inside one list still come back through `v-model:items`. See [useSortableGroup](../composables/useSortableGroup.md).

```vue
<script setup lang="ts">
import { ref } from 'vue'
import { Sortable, useSortableGroup } from 'vael-ui'

const lists = { todo: ref([{ id: 1, label: 'Write docs' }]), done: ref([{ id: 2, label: 'Ship' }]) }
const board = useSortableGroup({
  onTransfer: (value, from, to) => {
    const source = lists[from.groupId as keyof typeof lists].value
    const index = source.findIndex((card) => card.id === value)
    const [card] = source.splice(index, 1)
    lists[to.groupId as keyof typeof lists].value.splice(to.index, 0, card!)
  },
})
</script>

<template>
  <Sortable v-for="(items, id) in lists" :key="id" v-model:items="items.value" item-key="id" label-key="label"
    :group="board" :group-id="id" restrict-to-list preview-mode="clone" class="column" />
</template>
```

Fixed-height columns that scroll: put the height and `overflow-y: auto` on the Sortable (its `class` reaches the `.ui-sortable` root) and turn on `restrict-to-list` so the dragged card stays inside the column it's over. Leave it off when columns grow with their cards.

**Showing the placeholder.** With `preview-mode="clone"` a copy follows the pointer and the original row is the placeholder: it moves to where the card will land and carries `data-sortable-source`. Sortable hides it with `visibility: hidden`. Each row is a `.ui-sortable-item` `<li>` with a 1px `--ui-border` border, holding the drag handle and a `.ui-sortable-content` box with your `#item` content, so show it like this:

```css
.column :deep(.ui-sortable-item[data-sortable-source]) {
  visibility: visible;
  border-style: dashed;
  background: transparent;
  box-shadow: none;
}
.column :deep(.ui-sortable-item[data-sortable-source] > *) { opacity: 0.4; }
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`itemKey` | `keyof T \| undefined` | `'value'` | Property that holds each item's stable identity. `value` matches `MenuItemData`, `SelectItemData` and `TreeNode`.
`labelKey` | `keyof T \| undefined` | `'label'` | Property to announce, and to render when you don't pass an `#item` slot.
`axis` | `'x' \| 'y' \| undefined` | `'y'` | `'y'` reorders a column of rows; `'x'` reorders a row of items. Arrow keys follow the axis.
`canDrop` | `((details: SortableDropDetails) => boolean) \| undefined` |  | Runs repeatedly while you drag; return `false` to mark the target invalid. Keep it cheap.
`beforeDrop` | `((details: SortableDropDetails) => boolean \| Promise<boolean>) \| undefined` |  | Async check at drop time. Return `false`, or a promise of `false`, to cancel. Pairs with `confirmAction().result`.
`autoScroll` | `boolean \| undefined` | `true` | Scrolls the list, any scrollable ancestor, or the page while a drag nears its edge.
`restrictToList` | `boolean \| undefined` | `false` | For lists in a `group`: keeps the dragged row inside the part of the list it's over that's on screen, so it never hangs past a fixed-height column. Leave it off for lists that grow with their rows.
`disabled` | `boolean \| undefined` | `false` | Turns off dragging, so rows stay static.
`motion` | `SortableMotion \| undefined` | `true` | How it moves: its appearance, row by row (`{ initial: { item: { opacity: 0, y: 8 } }, stagger: 0.04 }`), and its states (`{ states: { dragging: { root: { scale: '1.01' } } } }`). `false` skips the built-in springs, so rows snap to their new slots. Use it when you drive the motion yourself.
`group` | `SortableGroupHandle \| undefined` |  | Handle from `useSortableGroup()`. Lists that share it, `<Sortable>` or `useSortable()`, let items cross between them.
`groupId` | `string \| number \| undefined` |  | This list's identity within `group`. Unset, Sortable assigns one.
`previewMode` | `"element" \| "clone" \| undefined` | `'element'` | What follows the pointer. `'element'` moves the real item; `'clone'` floats a copy from the start of the drag, for content that can't leave its layout. The original is the placeholder: it moves to where the row will land and carries `data-sortable-source` (hidden by default; style it to show it dimmed or outlined).
`touchDragDelay` | `number \| undefined` | `0` | Milliseconds a touch must hold a row still before a drag starts. Unset, drags start at once. Set it only when `#item` or `#handle` content is also tappable; the built-in handle never needs it.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; handle: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`items` | `T[] \| undefined` | `[]` | The list, in order. Sortable assigns a new array when a drop commits, so bind it with `v-model:items`.

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; index: number; grabbed: boolean; }` | Row content. Unset, the row shows the item's `labelKey` value.
`handle` | `{ item: T; }` | Replaces the default drag handle.

## Events

Name | Type | Description
--- | --- | ---
`drop-error` | `[error: unknown, details: SortableDropDetails]` | Fires when `beforeDrop` throws or rejects, after Sortable reverts the move.
`reorder` | `[value: string \| number, to: DropPosition]` | Fires after `items` changes order. Persist optimistically here; on failure, restore your own snapshot of `items`.
`update:items` | `[value: T[]]` | Fires when `items` changes (`v-model:items`).

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | Root element.
`isGrabbed` | `boolean` | Whether an item is held, by pointer or keyboard.
`isSettling` | `boolean` | Whether rows are still gliding into place after a drop or cancel. The root also carries `data-settling` then. A list that clips its overflow only while something is held would cut a row off mid-glide, so keep the clipping off while this is true too.
`isValidDrop` | `boolean` | `false` while hovering a target that `canDrop` rejected.
`isPending` | `boolean` | Whether an async `beforeDrop` is still deciding.
`isForeignDropTarget` | `boolean` | `group` only: whether a drag from another list is hovering this one.
`activeValue` | `string \| number \| null` | Key of the item being dragged, or `null`.
`parts` | `Record<string, HTMLElement \| HTMLElement[] \| null>` | Its elements by part name, for your own animation; a repeated part is an array.

## Motion

`motion` takes `SortableMotion` (exported from `'vael-ui'`); `false` turns it off. Sortable: its appearance, row by row, and states.

- Parts: `root`, `item`
- States: `dragging`, `settling`
- Keys: `spring`, `duration`, `easing`, `initial`, `states`, `replay`, `appear`, `stagger`

See the [Motion guide](../guides/motion.md) for what each key does.

