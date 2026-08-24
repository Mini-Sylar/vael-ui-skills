# useSortable

The spring-driven drag-to-reorder engine behind `<Sortable>`, `<Tree>`'s nested reorder, and `<DataTable>`'s column reorder — pointer and keyboard drive the same grabbed state, and all ordering/nesting decisions live in pure, independently-tested functions. `<Sortable>` is just a thin, optional convenience layer over this; reach for the composable directly when you need custom markup a component can't give you, or when building a cross-container board with `useSortableGroup()`, which wraps this same engine.

## Parameters

Name | Type | Description
--- | --- | ---
`rows` | `MaybeRefOrGetter<readonly FlatSortableRow[]>` | Visible rows in visual order — { value, depth, parentValue } — re-read at grab time.
`getElement` | `(value) => HTMLElement \| null` | Resolves a row to the DOM node the engine measures and transforms directly.
`onCommit` | `(value, to: DropPosition) => void` | Apply the reorder. Fires once, on a committed drop.
`axis` | `MaybeRefOrGetter<'y' \| 'x'>` | Default 'y'. Nesting is only meaningful on 'y'.
`nested` | `MaybeRefOrGetter<boolean>` | Enables depth changes — Tree turns this on, a flat list leaves it off.
`dropOnTarget` | `MaybeRefOrGetter<boolean>` | VS Code model: hovering a row's middle drops INTO it. Requires nested.
`reorderSiblings` | `MaybeRefOrGetter<boolean>` | false disables reordering among current siblings — only re-parenting is offered, with no indicator on a would-be sibling insert. Requires dropOnTarget.
`canNestInto` | `(value) => boolean` | Which rows accept children. Without this, every row does.
`childCountOf` | `(value) => number` | Existing child count, so an "inside" drop appends.
`dragPreview` | `MaybeRefOrGetter<boolean>` | Lifts the grabbed row out as a floating preview that follows the cursor, leaving its slot dimmed — without it the row stays in flow and slides over its neighbours. Forced on automatically when `group` is set.
`disabled` | `MaybeRefOrGetter<boolean>` | 
`motionCss` | `MaybeRefOrGetter<boolean>` | false disables the built-in springs — positions snap.
`canDrop` | `(details: SortableDropDetails) => boolean` | Synchronous structural veto, re-run while dragging: false marks the target invalid and blocks the drop. Keep it cheap.
`beforeDrop` | `(details) => boolean \| Promise<boolean>` | Async gate at drop time — return false (or a promise of it) to cancel and spring the item home. Composes with confirmAction().result.
`onDropError` | `(error, details) => void` | beforeDrop threw or rejected; the move is already reverted by the time this fires.
`labelOf / announce` | `(value) => string / (event) => string` | Human label and live-region text for assistive tech.
`group / groupId / container` | `SortableGroupHandle / string \| number / MaybeRefOrGetter<HTMLElement \| null>` | Shares drag sessions with other useSortable() lists passed the same handle — see useSortableGroup(). container is only needed when group is set, so it can hit-test an empty list.

## Returns

Name | Type | Description
--- | --- | ---
`activeValue / isGrabbed / isDragging` | `Ref` | Which row is held, and by which input (isDragging is pointer-only, never a plain click).
`isGrabbedValue` | `(value) => boolean` | Bind directly: :data-grabbed="isGrabbedValue(row.value) \|\| undefined". True for a folder's whole dragged subtree, not just the row you grabbed.
`dropPosition / isValidDrop / isPending` | `Ref` | Where it would land, whether canDrop currently allows that, and whether an async beforeDrop is still deciding.
`dropIntoValue / dropTargetValue / dropIntent` | `Ref` | Drop-on-target mode only: which row is being hovered, and before/after/inside.
`draggedValues` | `Ref<ReadonlySet>` | Every value in the dragged block — a folder carries its descendants.
`announcement` | `Ref<string>` | Live-region text. Render it in an aria-live="assertive" node.
`onHandlePointerdown / onHandleKeydown` | `(event, value) => void` | Wire directly to a row's handle element.
`consumeSuppressedClick` | `() => boolean` | True exactly once after a committed drag — swallow the trailing click a drag also triggers.
`cancel` | `() => void` | Abandon the current grab and spring everything home.

