# useSortableGroup

Moves items between lists: the building block for a Kanban-style board, not a component. `<Sortable>`, `<Tree>`, column reordering in `<DataTable>` and `v-draggable` all reorder within one list. This lets an item move from one `useSortable()` list into another, on the same spring-driven engine. Each list still calls `useSortable()`, or uses `<Sortable>` with the same `group` and `groupId` props. The group decides which list shows the open gap, and runs the transfer on drop. The list where the drag started keeps the pointer or keyboard gesture until the drag ends.

```ts
import { useSortableGroup } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`onTransfer` | `(value, from: GroupDropPosition, to: GroupDropPosition) => void` | Required. Fires once, on a committed drop into another list. Take the item at `from.index` out of the array for `from.groupId` and insert it at `to.index` in the array for `to.groupId`. Use the indexes: `value` is the item's key from a `Sortable`, but its position from `v-draggable`. It lives on the group, not on each list, because a move between lists isn't either list's decision.
`canDrop` | `(details: GroupDropDetails) => boolean` | Vetoes a move between lists while dragging, such as a WIP limit on the target column. It re-runs live, so keep it cheap.
`beforeDrop` | `(details: GroupDropDetails) => boolean \| Promise<boolean>` | An async check at drop time. Return `false`, or a promise of `false`, to cancel and spring the item back. Works with `confirmAction().result`, like `useSortable`'s `beforeDrop`.
`onDropError` | `(error: unknown, details: GroupDropDetails) => void` | `beforeDrop` threw or rejected. The move is already reverted when this fires.
`motion` | `MaybeRefOrGetter<boolean>` | `false` turns off the springs for the gap that opens in another list while you hover it.

## Returns

Name | Type | Description
--- | --- | ---
`join(options)` | `UseSortableReturn` | The usual way to add a list: `useSortable()` with `group` and `groupId` already set, so each column is one call. `groupId` is auto-assigned if you omit it. Pass your own so `onTransfer` knows which array to update.
