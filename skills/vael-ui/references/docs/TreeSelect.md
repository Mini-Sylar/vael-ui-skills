# TreeSelect

A dropdown whose panel is a Tree, for picking one or more nodes from a hierarchy.

```ts
import { TreeSelect } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | Tree data: nodes with `value`, `label` and optional `children`.
`placeholder` | `string \| undefined` |  | Text shown in the trigger while nothing is selected.
`selectionMode` | `TreeSelectionMode \| undefined` | `'single'` | `'single'`: a click replaces the selection and closes the panel. `'multiple'`: a click toggles that node only. `'checkbox'`: checkboxes with cascading parent/child toggles.
`selectableFolders` | `boolean \| undefined` | `true` | `false` makes folders unselectable, so only leaves can become the value. No effect in `'checkbox'` mode, which puts only leaves in the model.
`disabled` | `boolean \| undefined` | `false` | Disables the trigger and blocks interaction.
`clearable` | `boolean \| undefined` | `false` | Shows a clear button once something is selected.
`invalid` | `boolean \| undefined` | `false` | Marks the field invalid. ORed with the nearest Field's `error` state.
`size` | `"md" \| "sm" \| "lg" \| undefined` | `'md'` | Trigger size.
`filterable` | `boolean \| undefined` | `true` | Shows the built-in label search box atop the panel, auto-expanding ancestors of any match. `false` removes it entirely.
`filterPlaceholder` | `string \| undefined` | `'Search...'` | Placeholder for the search box.
`emptyText` | `string \| undefined` | `'No results found'` | Text shown when no nodes match the search or `items` is empty.
`expandOnRowClick` | `boolean \| undefined` | `false` | Clicking a folder row also toggles its expansion, not only the chevron. The click still selects the row unless `selectableFolders` is `false`.
`stickyScroll` | `boolean \| undefined` | `false` | Pins each expanded ancestor's row to the top of the panel while its children scroll past, VS Code-style. Nested rows keep their folder context in view.
`name` | `string \| undefined` |  | Renders hidden `<input>`(s) so a plain `<form>` post carries the selection, repeating `name` outside `'single'` mode.
`side` | `Side \| undefined` | `'bottom'` | Which side of the trigger the panel opens on.
`align` | `Align \| undefined` | `'start'` | How the panel aligns against the trigger along that side.
`sideOffset` | `number \| undefined` | `8` | Gap between the trigger and the panel, in pixels.
`alignOffset` | `number \| undefined` | `0` | Shifts the panel along the alignment axis, in pixels.
`closeOnEsc` | `boolean \| undefined` | `true` | Escape key closes the panel.
`closeOnOutside` | `boolean \| undefined` | `true` | Clicking outside the panel closes it.
`beforeClose` | `((done: () => void) => void) \| undefined` |  | Custom exit animation; call `done()` to finish closing.
`forceMount` | `boolean \| undefined` | `false` | Keeps it mounted, toggled with `v-show`, so you can own the enter/exit animation.
`teleportTo` | `string \| HTMLElement \| undefined` | `'body'` | Teleport target: a CSS selector or element.
`maxPanelHeight` | `number \| undefined` | `320` | Caps the panel height in pixels; the tree scrolls past it. The viewport limits it too, so pass `Infinity` to fill the available space.
`motionCss` | `boolean \| undefined` | `true` | `false` skips all built-in motion (row transitions and chevron rotation).
`ui` | `Partial<{ trigger: UiPartValue; value: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; filter: UiPartValue; list: UiPartValue; node: UiPartValue; empty: UiPartValue; footer: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | `null` | Selected value, or an array of values outside `'single'` mode.
`open` | `boolean \| undefined` | `false` | Whether the panel is open.
`query` | `string \| undefined` | `''` | Search box text.
`node` | `T \| T[] \| null \| undefined` | `null` | Selected node object(s), shaped like the model. It mirrors the model, so writes to it don't stick.

## Slots

Name | Type | Description
--- | --- | ---
`value` | `{ selected: T[]; }` | Custom trigger content for the selected nodes.
`header` | `any` | Content above the filter input (when `filterable` is on) or the tree.
`node` | `{ node: T; depth: number; expanded: boolean; checked: boolean; indeterminate: boolean; disabled: boolean; toggleExpand: () => void; toggleSelect: () => void; findNode: (value: string \| number) => T \| undefined; findParent: (value: string \| number) => T \| null; removeNode: (value: string \| number) => boolean; }` | Custom row content for each node; the built-in row wrapper and behavior stay in place.
`empty` | `any` | Replaces `emptyText`.
`footer` | `any` | Content below the tree.

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | Fires before the panel closes; `details.cancel()` keeps it open.
`select` | `[node: T]` | Fires when you pick or toggle a node.
`change` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when you change the selection, including clear.
`expand-change` | `[value: string \| number, expanded: boolean]` | Fires when a node expands or collapses, except during filter-driven auto-expansion.
`update:open` | `[value: boolean]` | Fires when `open` changes (`v-model:open`).
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | Fires when `modelValue` changes (`v-model`).
`update:query` | `[value: string]` | Fires when `query` changes (`v-model:query`).
`update:node` | `[value: T \| T[] \| null]` | Fires when `node` changes (`v-model:node`).

## Exposed

Name | Type | Description
--- | --- | ---
`triggerEl` | `HTMLElement \| null` | Trigger element.
`panelEl` | `HTMLElement \| null` | Panel element (null while closed).
`positionerEl` | `HTMLElement \| null` | Positioning wrapper around the panel (null while closed).
`listEl` | `HTMLElement \| null` | The tree's `role="tree"` element (null while closed).
`placement` | `Placement` | Resolved placement after flipping, e.g. `'bottom-start'`.
`positionerStyle` | `Record<string, string>` | Inline positioning styles applied to the positioner.
`isClosing` | `boolean` | True while a `beforeClose` close is pending.
`open` | `() => void` | Opens the panel (no-op while disabled).
`close` | `() => void` | Closes the panel, running `@open-change` and `beforeClose` first.
`cancelClose` | `() => void` | Cancels a close pending in `beforeClose` and keeps the panel open.
`expandAll` | `() => void \| undefined` | Expands every folder. No-op while the panel is closed.
`collapseAll` | `() => void \| undefined` | Collapses every folder. No-op while the panel is closed.
`expandNode` | `(value: string \| number) => void \| undefined` | Expands the node with this value. No-op while the panel is closed.
`collapseNode` | `(value: string \| number) => void \| undefined` | Collapses the node with this value. No-op while the panel is closed.
`findNode` | `(value: string \| number) => TreeNode \| undefined` | Finds the node with this value in `items` (`undefined` while closed).
`findParent` | `(value: string \| number) => TreeNode \| null` | Finds the parent of the node with this value (null for a root node or while closed).
`removeNode` | `(value: string \| number) => boolean` | Removes the node with this value from `items` in place; returns whether it was found.

