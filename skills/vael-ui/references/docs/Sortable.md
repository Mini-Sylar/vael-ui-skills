# Sortable

A drag-to-reorder list with a real keyboard path, not just a pointer one.

```ts
import { Sortable } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`itemKey` | `keyof T \| undefined` | "value" as never | Property holding each item's stable identity. Defaults to `value`, the same item vocabulary `MenuItemData`/`SelectItemData`/`TreeNode` already use.
`labelKey` | `keyof T \| undefined` | "label" as never | Property to announce and to render when no `#item` slot is given.
`axis` | `SortableAxis \| undefined` | "y" | `'y'` (default) reorders a column of rows; `'x'` reorders a row of items. Arrow keys follow the axis.
`canDrop` | `((details: SortableDropDetails) => boolean) \| undefined` | undefined | Structural veto, re-run while dragging: `false` marks the target invalid. Keep it cheap.
`beforeDrop` | `((details: SortableDropDetails) => boolean \| Promise<boolean>) \| undefined` | undefined | Async gate at drop time. Return `false` (or a promise of it) to cancel — composes with `confirmAction().result`.
`disabled` | `boolean \| undefined` | false | 
`motionCss` | `boolean \| undefined` | true | `false` skips the built-in springs entirely — rows snap to their new slots. Reach for it when driving the motion yourself.
`group` | `SortableGroupHandle \| undefined` | undefined | Shares drag sessions with other `<Sortable>`s/`useSortable()` lists passed the same handle — from `useSortableGroup()`. Lets an item cross between them.
`groupId` | `string \| number \| undefined` | undefined | This list's identity within `group`. Auto-assigned if omitted.
`ui` | `Partial<{ root: UiPartValue; item: UiPartValue; handle: UiPartValue; }> \| undefined` | undefined | 
`items` | `T[] \| undefined` | [] | 

## Slots

Name | Type | Description
--- | --- | ---
`item` | `{ item: T; index: number; grabbed: boolean; }` | 
`handle` | `{ item: T; }` | 

## Events

Name | Type | Description
--- | --- | ---
`reorder` | `[value: string \| number, to: DropPosition]` | 
`drop-error` | `[error: unknown, details: SortableDropDetails]` | 
`update:items` | `[value: T[]]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`isGrabbed` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`isValidDrop` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`isPending` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.
`activeValue` | `unknown` | Type inference unavailable — vue-component-meta cannot resolve defineExpose on this generic component.

