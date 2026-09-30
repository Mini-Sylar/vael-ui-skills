# Tree

A collapsible, indented list for hierarchical data with single or multi-selection.

```ts
import { Tree } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Tree data: `TreeNode` objects (`value`, `label`, optional `children`) or your own extension of them.
`selectionMode` | `TreeSelectionMode \| undefined` | `'single'` | `'single'`: a click replaces the selection. `'multiple'`: a click toggles that node only. `'checkbox'`: checkboxes with cascading parent/child toggles.
`selectableFolders` | `boolean \| undefined` | `true` | `false` makes folders unselectable, so only leaves can become the value. No effect in `selectionMode="checkbox"`, which only puts leaves in the model.
`filterable` | `boolean \| undefined` | `true` | Shows a built-in label search box above the tree and expands the ancestors of any match.
`filterPlaceholder` | `string \| undefined` | `'Search...'` | Placeholder and accessible label for the filter box.
`emptyText` | `string \| undefined` | `'No results found'` | Text shown when no rows are visible (empty `items` or no filter matches).
`motionCss` | `boolean \| undefined` | `true` | `false` skips all built-in motion (row transitions, chevron rotation and cross-folder moves).
`forceMount` | `boolean \| undefined` | `false` | Keeps collapsed folders' children mounted (`v-show` plus `data-state`) so you can animate expand/collapse yourself instead of using the built-in transition.
`reorderable` | `boolean \| undefined` | `false` | Lets you drag rows to reorder and nest them, VS Code style. Drop on a row's middle to move into it, or on an edge to place beside it.
`canNestInto` | `((node: T) => boolean) \| undefined` |  | Which rows accept dropped children. Unset, any row that already has children does; pass your own to let empty folders take drops.
`reorderSiblings` | `boolean \| undefined` | `true` | `false` turns off sibling reordering, so dragging only moves rows into a folder. Rows that can't hold children show no indicator.
`canDrop` | `((details: SortableDropDetails) => boolean) \| undefined` |  | Structural check that runs as you drag; returning `false` marks the target invalid.
`beforeDrop` | `((details: SortableDropDetails) => boolean \| Promise<boolean>) \| undefined` |  | Async check at drop time; return `false` (or a promise of it) to cancel. Works with `confirmAction().result`.
`autoExpandDelay` | `number \| undefined` | `600` | How long, in ms, you hover a collapsed row mid-drag before it opens.
`previewMode` | `"element" \| "clone" \| undefined` | `'clone'` | Drag preview: `'clone'` shows a floating copy while the real row hides until drop; `'element'` moves the real row itself.
`touchDragDelay` | `number \| undefined` | `150` | How long, in ms, a touch pointer must hold a row still before a drag starts. The hold tells a drag apart from a tap to select or expand. Mouse and pen are unaffected.
`expandOnRowClick` | `boolean \| undefined` | `false` | Clicking anywhere on a folder row toggles it, as well as the chevron. The click still selects the folder unless `selectableFolders` is `false`.
`stickyScroll` | `boolean \| undefined` | `false` | Pins each expanded ancestor row to the top of the list while its children scroll past, VS Code-style.
`id` | `string \| undefined` |  | Id for the `role="tree"` list element. Auto-generated when omitted.
`ui` | `Partial<{ list: UiPartValue; node: UiPartValue; filter: UiPartValue; empty: UiPartValue; chevron: UiPartValue; label: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | `null` | Selected value: one value in `'single'` mode, an array in `'multiple'`, leaf values in `'checkbox'`.
`query` | `string \| undefined` | `''` | Filter box text.
`node` | `T \| T[] \| null \| undefined` | `null` | Selected node object(s), resolved from `v-model` against `items`. Read-only in effect: the value model overwrites any writes.

## Slots

Name | Type | Description
--- | --- | ---
`node` | `{ node: T; depth: number; expanded: boolean; checked: boolean; indeterminate: boolean; disabled: boolean; toggleExpand: () => void; toggleSelect: () => void; findNode: (value: string \| number) => T \| undefined; findParent: (value: string \| number) => T \| null; removeNode: (value: string \| number) => boolean; }` | Row content inside the `role="treeitem"` wrapper. `findNode`/`findParent`/`removeNode` are `findTreeNode`/`findTreeParent`/`removeTreeNode` bound to this tree's `items`.
`empty` | `any` | Replaces the empty-state row shown when no rows are visible.

## Events

Name | Type | Description
--- | --- | ---
`select` | `[node: T]` | Fires when you select or toggle a node.
`change` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when you change the selection, with the new model value.
`drop-error` | `[error: unknown, details: SortableDropDetails]` | Fires when `beforeDrop` throws or rejects, after the move is reverted.
`expand-change` | `[value: string \| number, expanded: boolean]` | Fires when a single node expands or collapses. Filter auto-expansion, `expandAll` and `collapseAll` don't fire it.
`reorder` | `[value: string \| number, to: DropPosition]` | Fires after `items` has been reordered in place.
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when `modelValue` changes (`v-model`).
`update:query` | `[value: string]` | Fires when `query` changes (`v-model:query`).
`update:node` | `[value: T \| T[] \| null]` | Fires when `node` changes (`v-model:node`).

## Exposed

Name | Type | Description
--- | --- | ---
`listEl` | `HTMLElement \| null` | The `role="tree"` list element.
`filterInputRef` | `{ el: HTMLElement \| null; inputEl: HTMLInputElement \| null; } \| null` | The filter box's Input instance (null when `filterable` is off).
`focusFirstRow` | `() => void` | Focuses the first visible row.
`initRoving` | `() => void` | Makes the first visible row the tab stop without focusing it.
`expandAll` | `() => void` | Expands every folder.
`collapseAll` | `() => void` | Collapses every folder.
`expandNode` | `(value: string \| number) => void` | Expands the node with this value, e.g. after adding a child to it.
`collapseNode` | `(value: string \| number) => void` | Collapses the node with this value.
`findNode` | `(value: string \| number) => T \| undefined` | Finds a node in `items` by value.
`findParent` | `(value: string \| number) => T \| null` | Finds a node's parent in `items` by value (null at the root).
`removeNode` | `(value: string \| number) => boolean` | Removes a node from `items` in place; returns whether one was found.
`isReordering` | `boolean` | `true` while a row is held, by pointer or keyboard.
`cancelReorder` | `() => void` | Aborts an in-flight reorder and springs every row back into place.

