# useSortable

The drag-to-reorder engine behind `<Sortable>`, nested reordering in `<Tree>` and column reordering in `<DataTable>`, with rows that move on springs. Pointer and keyboard input drive the same grabbed state. Pure, separately tested functions make every ordering and nesting decision. `<Sortable>` is an optional wrapper around it; use the composable when you need markup a component can't give you. To move items between lists, use `useSortableGroup()`, which wraps the same engine.

```ts
import { useSortable } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`rows` | `MaybeRefOrGetter<readonly FlatSortableRow[]>` | The visible rows in visual order, as `{ value, depth, parentValue }`. It is read again each time a row is grabbed.
`getElement` | `(value) => HTMLElement \| null` | Returns the DOM node for a row. The engine measures and moves that node directly.
`onCommit` | `(value, to: DropPosition) => void` | Applies the reorder. It fires once, on a committed drop.
`axis` | `MaybeRefOrGetter<'y' \| 'x'>` | The axis rows reorder along. Default `'y'`. Nesting only works on `'y'`. Rows that wrap onto more lines (a card grid) reorder in reading order, slot by slot.
`nested` | `MaybeRefOrGetter<boolean>` | Allows depth changes. `Tree` turns it on; a flat list leaves it off.
`dropOnTarget` | `MaybeRefOrGetter<boolean>` | Drops into a row, as in VS Code: hovering the middle of a row drops the item inside it. Requires `nested`.
`reorderSiblings` | `MaybeRefOrGetter<boolean>` | `false` turns off reordering among current siblings. Only re-parenting is offered, and no indicator shows for a sibling insert. Requires `dropOnTarget`.
`nestEdgeFraction` | `MaybeRefOrGetter<number>` | Sets the share of a target's size, at each end, that means "beside it" rather than "into it". Default `0.25`, which leaves a 50% inside zone. Lower it for targets that are hard to hit.
`canNestInto` | `(value) => boolean` | Says which rows accept children. Without it, every row does.
`childCountOf` | `(value) => number` | Returns the child count, so an "inside" drop appends to the end.
`dragPreview` | `MaybeRefOrGetter<boolean>` | Lifts the grabbed row into a floating preview that follows the cursor, and dims its slot. Without it, the row stays in the layout and slides past its neighbors.
`previewMode` | `MaybeRefOrGetter<'element' \| 'clone'>` | What floats while you drag. `'element'` lifts the real row, so only one copy is on screen. `'clone'` floats a separate copy and hides the real row until the drop. Use `'clone'` when the real element can't leave its layout, such as a `<tr>` or `<th>`. The default depends on context: `'element'` once a `group` drag leaves this list, `'clone'` for a plain `dragPreview` drag.
`previewCarriesSubtree` | `MaybeRefOrGetter<boolean>` | Moves the dragged row's visible descendants, such as an expanded folder, inside the floating clone at their current offsets, so the whole block lifts as one piece. Default `true`. Only with `dragPreview` and `nested`.
`disabled` | `MaybeRefOrGetter<boolean>` | Turns off dragging.
`motion` | `MaybeRefOrGetter<boolean>` | `false` turns off the built-in springs, so rows snap into place.
`canDrop` | `(details: SortableDropDetails) => boolean` | Vetoes a drop target while dragging. Returning `false` marks the target invalid and blocks the drop. It runs again on every move, so keep it cheap.
`beforeDrop` | `(details) => boolean \| Promise<boolean>` | Checks a drop before it happens. Return `false`, or a promise of `false`, to cancel and spring the item back. Works with `confirmAction().result`.
`onDropError` | `(error, details) => void` | Fires when `beforeDrop` throws or rejects. The move is already reverted by then.
`labelOf / announce` | `(value) => string / (event) => string` | Return the row label and the live-region text for screen readers.
`group / groupId / container` | `SortableGroupHandle / string \| number / MaybeRefOrGetter<HTMLElement \| null>` | Shares drag sessions with other `useSortable()` lists that get the same handle. See `useSortableGroup()`. `container` is only needed with `group`, so a drag can find an empty list.
`restrictToList` | `MaybeRefOrGetter<boolean>` | Keeps a floating row inside the part of the list it's over that's on screen, so it never hangs past a fixed-height column. Default `false`.

## Returns

Name | Type | Description
--- | --- | ---
`activeValue / isGrabbed / isDragging` | `Ref` | The held row and how it's held. `isDragging` is pointer-only and stays `false` for a plain click.
`isSettling` | `Ref<boolean>` | `true` from a drop or cancel until every row reaches its slot. A list that clips its overflow only while `isGrabbed` should stay open while this is true too.
`isGrabbedValue` | `(value) => boolean` | Says whether a row is part of the drag: `:data-grabbed="isGrabbedValue(row.value) \|\| undefined"`. It's `true` for a folder's whole dragged subtree, not only the grabbed row.
`dropPosition / isValidDrop / isPending` | `Ref` | Where the item would drop, whether `canDrop` allows it, and whether an async `beforeDrop` is still pending.
`dropIntoValue / dropTargetValue / dropIntent` | `Ref` | Drop-on-target mode only: the hovered row, and whether the drop goes before, after or inside it.
`draggedValues` | `Ref<ReadonlySet>` | Every value in the dragged block, including a folder's descendants.
`isForeignDropTarget` | `Ref<boolean>` | `true` while a drag from another list in the group hovers this list. Always `false` for the list the drag started in. Use it to style drop-target feedback. Only with `group`.
`announcement` | `Ref<string>` | The text for screen readers. Render it in an `aria-live="polite"` element.
`onHandlePointerdown / onHandleKeydown` | `(event, value) => void` | Bind these to a row's handle element.
`consumeSuppressedClick` | `() => boolean` | Returns `true` once after a committed drag, so you can ignore the click that follows it.
`cancel` | `() => void` | Cancels the current grab and springs everything back.

