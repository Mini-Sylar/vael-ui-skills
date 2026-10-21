# moveTreeNode

Moves for a reorderable tree. `moveTreeNode` moves a node and its subtree to a new parent and index, mutating your array in place, the same contract as `removeTreeNode`. Its `to` argument is the `DropPosition` that `useSortable`'s `onCommit` gives you, so the two connect directly. Tree commits its drag reordering through it, and Sortable uses it for flat lists. `isDescendantOf` is the guard it runs first, because dropping a folder into its own descendant would detach that branch and lose it. Nodes are read as `{ value, children }` by default, the shape `TreeNode` and `MenuItemData` already have. Pass accessors for any other shape.

```ts
import { moveTreeNode, isDescendantOf } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```ts
import { ref } from 'vue'
import { isDescendantOf, moveTreeNode } from 'vael-ui'
import type { DropPosition, TreeAccessors } from 'vael-ui'

interface Folder {
  id: number
  name: string
  folders?: Folder[]
}

const tree = ref<Folder[]>([
  { id: 1, name: 'Projects', folders: [{ id: 2, name: 'Drafts' }] },
  { id: 3, name: 'Archive' },
])

const accessors: TreeAccessors<Folder> = {
  getKey: (folder) => folder.id,
  getChildren: (folder) => folder.folders,
  setChildren: (folder, folders) => (folder.folders = folders),
}

// Pass this as useSortable's onCommit, or call it from your own drop handler.
function onCommit(value: string | number, to: DropPosition) {
  moveTreeNode(tree.value, value, to, accessors)
}

// Drafts becomes Archive's first child. Archive had no children array, so one is created.
moveTreeNode(tree.value, 2, { parentValue: 3, index: 0, depth: 1 }, accessors)

isDescendantOf(tree.value, 3, 2, accessors) // true: Drafts is now inside Archive
```

## Parameters

Name | Type | Description
--- | --- | ---
`nodes` | `T[]` | The top-level array. `moveTreeNode` changes it, and the arrays inside it, in place.
`value` | `string \| number` | The key of the node to move.
`to` | `DropPosition` | `{ parentValue, index, depth }`. `parentValue` is the new parent's key, or `null` for the top level. `index` is the node's final slot among its new siblings, counted as if the node were already removed, and is clamped to the array. `depth` is not read.
`accessors` | `TreeAccessors<T>` | Optional. `getKey` reads a node's key, `node.value` by default. `getChildren` reads its children, `node.children` by default. `setChildren` attaches a new children array, used when the new parent has none yet. By default it assigns `node.children`.

## Returns

Name | Type | Description
--- | --- | ---
`moveTreeNode(nodes, value, to, accessors?)` | `boolean` | `true` when the node moved. `false` when nothing moved: the node or the parent wasn't found, or the parent is the node itself or inside its subtree. Check it before you emit or save anything.
`isDescendantOf(nodes, ancestorValue, candidateValue, accessors?)` | `boolean` | `true` when `candidateValue` is anywhere under `ancestorValue`. A node is not its own descendant. Use it to guard a drop handler of your own.
