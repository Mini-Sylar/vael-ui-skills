# Tree

A collapsible, indented list for hierarchical data with single or multi-selection.

```ts
import { Tree } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | 
`selectionMode` | `TreeSelectionMode \| undefined` | "single" | `'single'`: clicking replaces the selection. `'multiple'`: clicking toggles that node only. `'checkbox'`: checkboxes with cascading parent/child toggles.
`selectableFolders` | `boolean \| undefined` | true | `false` keeps a node with children out of the selection entirely — click, keyboard Enter/Space, and expandOnRowClick's own select-on-expand all skip it, only a leaf can become the value. Has no effect in `selectionMode="checkbox"`, which already only ever puts leaves in the model. Default: true (a folder can be selected like any other node).
`filterable` | `boolean \| undefined` | true | Shows a built-in label search box atop the tree, auto-expanding ancestors of any match. Default: on.
`filterPlaceholder` | `string \| undefined` | "Search..." | 
`emptyText` | `string \| undefined` | "No results found" | 
`motionCss` | `boolean \| undefined` | true | `false` skips all built-in motion (row transitions, chevron rotation, and cross-folder move).
`expandOnRowClick` | `boolean \| undefined` | false | When true, clicking anywhere on a folder row also toggles its expansion, not just the chevron — it still selects too (unless `selectableFolders` is off), so picking the folder itself (without opening it to reach a file inside) still works. Off by default since it changes what a plain row click does.
`stickyScroll` | `boolean \| undefined` | false | When true, each expanded ancestor's row pins to the top of the list as its own children scroll past, VS Code-style, so deeply nested content never loses its folder context. Uses native `position: sticky` — each row is a real, nested DOM level, not a JS-measured overlay.
`id` | `string \| undefined` | undefined | Id for the role="tree" list element. Auto-generated if omitted.
`ui` | `Partial<{ list: UiPartValue; node: UiPartValue; filter: UiPartValue; empty: UiPartValue; chevron: UiPartValue; label: UiPartValue; }> \| undefined` | undefined | 
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | null | 
`query` | `string \| undefined` | "" | 
`node` | `T \| T[] \| null \| undefined` | null | 

## Slots

Name | Type | Description
--- | --- | ---
`node` | `{ node: T; depth: number; expanded: boolean; checked: boolean; indeterminate: boolean; disabled: boolean; toggleExpand: () => void; toggleSelect: () => void; findNode: (value: string \| number) => T \| undefined; findParent: (value: string \| number) => T \| null; removeNode: (value: string \| number) => boolean; }` | Row content (inside the library's role="treeitem" wrapper). findNode/findParent/removeNode are shorthand for findTreeNode/findTreeParent/removeTreeNode bound to this instance's own `items`, for the common case of looking up a sibling/parent/self without importing them.
`empty` | `any` | Replaces the default empty-state row shown when nothing survives the filter.

## Events

Name | Type | Description
--- | --- | ---
`select` | `[node: T]` | 
`change` | `[value: string \| number \| (string \| number)[] \| null]` | 
`expand-change` | `[value: string \| number, expanded: boolean]` | 
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | 
`update:query` | `[value: string]` | 
`update:node` | `[value: T \| T[] \| null]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`listEl` | `HTMLElement \| null` | The `role="tree"` root element.
`filterInputRef` | `{ el: HTMLElement \| null; inputEl: HTMLInputElement \| null } \| null` | The built-in filter `Input`'s own exposed refs (only meaningful when `filterable` is on).
`focusFirstRow` | `() => void` | Moves roving-tabindex focus to the first visible row.
`initRoving` | `() => void` | Re-seeds roving tabindex to the first row — rarely needed directly, since `Tree` already re-seeds itself when the focused row leaves the DOM.
`expandAll` | `() => void` | Expands every node that has children.
`collapseAll` | `() => void` | Collapses every expanded node.
`expandNode` | `(value: string \| number) => void` | Expands a single node by value. No-op if already expanded or the node has no children.
`collapseNode` | `(value: string \| number) => void` | Collapses a single node by value. No-op if already collapsed.
`findNode` | `(value: string \| number) => T \| undefined` | Depth-first search for a node by value, scoped to this instance's own `items` — same lookup as the standalone `findTreeNode` export, pre-bound.
`findParent` | `(value: string \| number) => T \| null` | Depth-first search for a node's parent by the child's value, scoped to this instance's own `items` — pre-bound `findTreeParent`.
`removeNode` | `(value: string \| number) => boolean` | Removes a node by value (mutates `items`/its nested `children` arrays in place). Returns whether a match was found and removed — pre-bound `removeTreeNode`.

