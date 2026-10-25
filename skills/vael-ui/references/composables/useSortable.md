# useSortable

The spring-driven drag-to-reorder engine behind `<Sortable>`, nested reordering in `<Tree>` and column reordering in `<DataTable>`. Pointer and keyboard input drive the same grabbed state. Pure, separately tested functions make every ordering and nesting decision. `<Sortable>` is an optional wrapper around it. Use the composable directly when you need markup a component can't give you. To move items between lists, use `useSortableGroup()`, which wraps the same engine.

```ts
import { useSortable } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`rows` | `MaybeRefOrGetter<readonly FlatSortableRow[]>` | Visible rows in visual order, as `{ value, depth, parentValue }`. Read again each time a row is grabbed.
`getElement` | `(value) => HTMLElement \| null` | Returns the DOM node for a row. The engine measures and moves that node directly.
`onCommit` | `(value, to: DropPosition) => void` | Applies the reorder. Fires once, on a committed drop.
`axis` | `MaybeRefOrGetter<'y' \| 'x'>` | Default `'y'`. Nesting only works on `'y'`.
`nested` | `MaybeRefOrGetter<boolean>` | Enables depth changes. `Tree` turns it on; a flat list leaves it off.
`dropOnTarget` | `MaybeRefOrGetter<boolean>` | VS Code-style drops: hovering the middle of a row drops the item into it. Requires `nested`.
`reorderSiblings` | `MaybeRefOrGetter<boolean>` | `false` turns off reordering among current siblings. Only re-parenting is offered, and no indicator shows for a sibling insert. Requires `dropOnTarget`.
`nestEdgeFraction` | `MaybeRefOrGetter<number>` | The fraction of a target's size, at each end, that means "beside it" rather than "into it". Default `0.25`, which leaves a 50% inside zone. Lower it for targets that are hard to hit.
`canNestInto` | `(value) => boolean` | Which rows accept children. Without it, every row does.
`childCountOf` | `(value) => number` | Existing child count, so an "inside" drop appends to the end.
`dragPreview` | `MaybeRefOrGetter<boolean>` | Lifts the grabbed row into a floating preview that follows the cursor, and dims its slot. Without it, the row stays in the layout and slides past its neighbours.
`previewMode` | `MaybeRefOrGetter<'element' \| 'clone'>` | `'element'` lifts the real row, so only one copy is on screen. `'clone'` shows a separate floating copy and hides the real row until the drop. Use `'clone'` when the real element can't leave its layout, such as a `<tr>` or `<th>`. The default depends on context: `'element'` once a `group` drag leaves this list, `'clone'` for a plain `dragPreview` drag.
`previewCarriesSubtree` | `MaybeRefOrGetter<boolean>` | Only with `dragPreview` and `nested`. Default `true`. The dragged row's visible descendants (an expanded folder, a tab group) travel inside the floating clone at their current offsets. The whole block lifts as one piece.
`disabled` | `MaybeRefOrGetter<boolean>` | Turns off dragging.
`motion` | `MaybeRefOrGetter<boolean>` | `false` turns off the built-in springs, so positions snap.
`canDrop` | `(details: SortableDropDetails) => boolean` | A synchronous veto, re-run while dragging. Returning `false` marks the target invalid and blocks the drop. Keep it cheap.
`beforeDrop` | `(details) => boolean \| Promise<boolean>` | An async check at drop time. Return `false`, or a promise of `false`, to cancel and spring the item back. Works with `confirmAction().result`.
`onDropError` | `(error, details) => void` | `beforeDrop` threw or rejected. The move is already reverted when this fires.
`labelOf / announce` | `(value) => string / (event) => string` | The row label and live-region text for assistive tech.
`group / groupId / container` | `SortableGroupHandle / string \| number / MaybeRefOrGetter<HTMLElement \| null>` | Shares drag sessions with other `useSortable()` lists that get the same handle. See `useSortableGroup()`. `container` is only needed with `group`, so an empty list can be hit-tested.

## Returns

Name | Type | Description
--- | --- | ---
`activeValue / isGrabbed / isDragging` | `Ref` | The held row and how it's held. `isDragging` is pointer-only and stays `false` for a plain click.
`isGrabbedValue` | `(value) => boolean` | Bind it directly: `:data-grabbed="isGrabbedValue(row.value) \|\| undefined"`. `true` for a folder's whole dragged subtree, not only the grabbed row.
`dropPosition / isValidDrop / isPending` | `Ref` | Where the item would land, whether `canDrop` allows it, and whether an async `beforeDrop` is still pending.
`dropIntoValue / dropTargetValue / dropIntent` | `Ref` | Drop-on-target mode only: the hovered row, and whether the drop goes before, after or inside it.
`draggedValues` | `Ref<ReadonlySet>` | Every value in the dragged block. A folder carries its descendants.
`isForeignDropTarget` | `Ref<boolean>` | Only with `group`. `true` while a drag from another list in the group hovers this list. Always `false` for the list the drag started in. Use it to style drop-target feedback.
`announcement` | `Ref<string>` | Live-region text. Render it in an `aria-live="assertive"` element.
`onHandlePointerdown / onHandleKeydown` | `(event, value) => void` | Bind these to a row's handle element.
`consumeSuppressedClick` | `() => boolean` | Returns `true` once after a committed drag. Use it to ignore the click that follows the drag.
`cancel` | `() => void` | Abandons the current grab and springs everything back.
