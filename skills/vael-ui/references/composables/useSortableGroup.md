# useSortableGroup

Cross-container drag — the primitive a Kanban-style board is built from, not a component. `<Sortable>`, `<Tree>`, `<DataTable>`'s column reorder, and `v-draggable` all reorder within one list; this is what lets an item cross from one `useSortable()` list into another, over the exact same spring-driven engine. Each list still calls `useSortable()` itself (or `<Sortable>`, which takes the same `group`/`groupId` props directly) — the group only decides which one currently shows the open gap, and runs the actual transfer on drop. The origin list keeps full ownership of the pointer/keyboard gesture for the whole drag; nothing is ever handed off mid-flight.

## Parameters

Name | Type | Description
--- | --- | ---
`onTransfer` | `(value, from: GroupDropPosition, to: GroupDropPosition) => void` | The only required option. Fires once, on a committed cross-container drop — splice `value` out of the array named by `from.groupId`, into `to.groupId`. Lives only here, never duplicated per-list, since a cross-boundary decision has no coherent meaning as one column’s opinion versus another’s.
`canDrop` | `(details: GroupDropDetails) => boolean` | Vetoes a cross-container move while dragging — a WIP limit on the target column, say. Re-run live; keep it cheap.
`beforeDrop` | `(details: GroupDropDetails) => boolean \| Promise<boolean>` | Async gate at drop time. Return false (or a promise of it) to cancel and spring the item back home — composes with confirmAction().result exactly like useSortable’s own beforeDrop.
`onDropError` | `(error: unknown, details: GroupDropDetails) => void` | beforeDrop threw or rejected; the move is already reverted by the time this fires.
`motionCss` | `MaybeRefOrGetter<boolean>` | false skips the springs for the ghost gap opened in a foreign column while hovering it.

## Returns

Name | Type | Description
--- | --- | ---
`join(options)` | `UseSortableReturn` | The ergonomic default: useSortable() with group/groupId already wired in, so a column is one call instead of two things to keep consistent by hand. groupId is optional (auto-assigned if omitted), but a real one is what onTransfer receives to know which array/branch it’s dealing with.

