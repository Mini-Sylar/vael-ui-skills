# useSortableGroup

Moves items between lists, the building block for a Kanban-style board. `<Sortable>`, `<Tree>`, column reordering in `<DataTable>` and `v-draggable` reorder within one list; this lets an item move from one `useSortable()` list into another, on the same engine. Each list still calls `useSortable()`, or uses `<Sortable>` with the same `group` and `groupId` props. The group decides which list shows the open gap, and runs the transfer on drop. The list where the drag started keeps the pointer or keyboard gesture until the drag ends.

```ts
import { useSortableGroup } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`onTransfer` | `(value, from: GroupDropPosition, to: GroupDropPosition) => void` | Moves the item between your arrays. It fires once, on a committed drop into another list: take the item at `from.index` out of the array for `from.groupId`, and insert it at `to.index` in the array for `to.groupId`. Use the indexes: `value` is the item's key from a `Sortable`, but its position from `v-draggable`, which has no keys. Required. It belongs to the group, not to each list, because a move between lists isn't either list's decision.
`canDrop` | `(details: GroupDropDetails) => boolean` | Vetoes a move between lists while dragging, such as a work-in-progress limit on the target column. It runs again on every move, so keep it cheap.
`beforeDrop` | `(details: GroupDropDetails) => boolean \| Promise<boolean>` | Checks a drop before it happens. Return `false`, or a promise of `false`, to cancel and spring the item back. Works with `confirmAction().result`, like `useSortable`'s `beforeDrop`.
`onDropError` | `(error: unknown, details: GroupDropDetails) => void` | Fires when `beforeDrop` throws or rejects. The move is already reverted by then.
`motion` | `MaybeRefOrGetter<boolean>` | `false` turns off the springs for the gap that opens in another list while you hover it.

## Returns

Name | Type | Description
--- | --- | ---
`join(options)` | `UseSortableReturn` | Adds a list to the group: `useSortable()` with `group` and `groupId` already set, so each column is one call. Without a `groupId`, it assigns one; pass your own so `onTransfer` knows which array to update.

