# Sortable

A drag-to-reorder list with a real keyboard path, not just a pointer one.

```ts
import { Sortable } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`itemKey` | `keyof T \| undefined` | `'value'` | Property that holds each item's stable identity. `value` matches `MenuItemData`, `SelectItemData` and `TreeNode`.
`labelKey` | `keyof T \| undefined` | `'label'` | Property to announce, and to render when you don't pass an `#item` slot.
`axis` | `SortableAxis \| undefined` | `'y'` | `'y'` reorders a column of rows; `'x'` reorders a row of items. Arrow keys follow the axis.
`canDrop` | `((details: SortableDropDetails) => boolean) \| undefined` |  | Runs repeatedly while you drag; return `false` to mark the target invalid. Keep it cheap.
`beforeDrop` | `((details: SortableDropDetails) => boolean \| Promise<boolean>) \| undefined` |  | Async check at drop time. Return `false`, or a promise of `false`, to cancel. Pairs with `confirmAction().result`.
`autoScroll` | `boolean \| undefined` | `true` | Scrolls the list, any scrollable ancestor, or the page while a drag nears its edge.
`disabled` | `boolean \| undefined` | `false` | Turns off dragging, so rows stay static.
`motionCss` | `boolean \| undefined` | `true` | `false` skips the built-in springs, so rows snap to their new slots. Use it when you drive the motion yourself.
`group` | `SortableGroupHandle \| undefined` |  | Handle from `useSortableGroup()`. Lists that share it, `<Sortable>` or `useSortable()`, let items cross between them.
`groupId` | `string \| number \| undefined` |  | This list's identity within `group`. Unset, Sortable assigns one.
`previewMode` | `"element" \| "clone" \| undefined` |  | `group` only: what follows the pointer once a drag leaves this list. `'element'` moves the real item; `'clone'` floats a copy, for content that can't leave its layout.
`touchDragDelay` | `number \| undefined` |  | Milliseconds a touch must hold a row still before a drag starts. Unset, drags start at once. Set it only when `#item` or `#handle` content is also tappable; the built-in handle never needs it.
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
`isValidDrop` | `boolean` | `false` while hovering a target that `canDrop` rejected.
`isPending` | `boolean` | Whether an async `beforeDrop` is still deciding.
`isForeignDropTarget` | `boolean` | `group` only: whether a drag from another list is hovering this one.
`activeValue` | `string \| number \| null` | Key of the item being dragged, or `null`.

