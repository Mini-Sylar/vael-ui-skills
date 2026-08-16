# TreeSelect

A dropdown whose panel is a Tree, for picking one or more nodes from a hierarchy.

```ts
import { TreeSelect } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`items` | `readonly T[]` |  | 
`placeholder` | `string \| undefined` | undefined | 
`selectionMode` | `TreeSelectionMode \| undefined` | "single" | `'single'`: clicking replaces selection and closes the panel. `'multiple'`: clicking toggles that node only. `'checkbox'`: checkboxes with cascading parent/child toggles.
`selectableFolders` | `boolean \| undefined` | true | `false` keeps a node with children out of the selection entirely — click, keyboard Enter/Space, and expandOnRowClick's own select-on-expand all skip it, only a leaf can become the value. Has no effect in `selectionMode="checkbox"`, which already only ever puts leaves in the model. Default: true (a folder can be selected like any other node).
`disabled` | `boolean \| undefined` | false | 
`clearable` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`filterable` | `boolean \| undefined` | true | Shows the built-in label search box atop the panel, auto-expanding ancestors of any match. `false` removes it entirely.
`filterPlaceholder` | `string \| undefined` | "Search..." | 
`emptyText` | `string \| undefined` | "No results found" | 
`expandOnRowClick` | `boolean \| undefined` | false | When true, clicking anywhere on a folder row also toggles its expansion, not just the chevron — it still selects too (unless `selectableFolders` is off), so picking the folder itself (without opening it to reach a file inside) still works. Off by default since it changes what a plain row click does.
`stickyScroll` | `boolean \| undefined` | false | When true, each expanded ancestor's row pins to the top of the panel as its own children scroll past, VS Code-style, so deeply nested content never loses its folder context.
`name` | `string \| undefined` | undefined | Renders hidden `<input>`(s) mirroring the selection, for plain `<form>` posts — repeated `name` outside `single` mode.
`side` | `Side \| undefined` | "bottom" | 
`align` | `Align \| undefined` | "start" | 
`sideOffset` | `number \| undefined` | 8 | 
`alignOffset` | `number \| undefined` | 0 | 
`closeOnEsc` | `boolean \| undefined` | true | 
`closeOnOutside` | `boolean \| undefined` | true | 
`beforeClose` | `((done: () => void) => void) \| undefined` | undefined | 
`forceMount` | `boolean \| undefined` | false | 
`teleportTo` | `string \| HTMLElement \| undefined` | "body" | 
`motionCss` | `boolean \| undefined` | true | `false` skips all built-in motion (row transitions and chevron rotation).
`ui` | `Partial<{ trigger: UiPartValue; value: UiPartValue; positioner: UiPartValue; panel: UiPartValue; filter: UiPartValue; list: UiPartValue; node: UiPartValue; empty: UiPartValue; }> \| undefined` | undefined | 
`modelValue` | `string \| number \| (string \| number)[] \| null \| undefined` | null | 
`open` | `boolean \| undefined` | false | 
`query` | `string \| undefined` | "" | 
`node` | `T \| T[] \| null \| undefined` | null | Mirrors `model`'s value(s) as the full node object(s) — see Tree.vue's own `node` model for the resolution details; TreeSelect just forwards it straight through from the inner Tree.

## Slots

Name | Type | Description
--- | --- | ---
`value` | `{ selected: T[]; }` | 
`node` | `{ node: T; depth: number; expanded: boolean; checked: boolean; indeterminate: boolean; disabled: boolean; toggleExpand: () => void; toggleSelect: () => void; findNode: (value: string \| number) => T \| undefined; findParent: (value: string \| number) => T \| null; removeNode: (value: string \| number) => boolean; }` | Row content override (library owns wrapper & behavior).
`empty` | `any` | 

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`select` | `[node: T]` | 
`change` | `[value: string \| number \| (string \| number)[] \| null]` | 
`expand-change` | `[value: string \| number, expanded: boolean]` | 
`update:open` | `[value: boolean]` | 
`update:modelValue` | `[value: string \| number \| (string \| number)[] \| null]` | 
`update:query` | `[value: string]` | 
`update:node` | `[value: T \| T[] \| null]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`triggerEl` | `HTMLElement \| null` | The trigger button element.
`panelEl` | `HTMLElement \| null` | The dropdown panel element.
`positionerEl` | `HTMLElement \| null` | The floating-positioned wrapper around the panel.
`listEl` | `HTMLElement \| null` | The inner `Tree`'s own `role="tree"` root element.
`placement` | `Placement` | The panel's resolved floating-ui placement (post auto-flip/shift).
`positionerStyle` | `Record<string, string>` | Inline styles floating-ui computes for the positioner.
`isClosing` | `boolean` | True while a `beforeClose` exit animation is in flight.
`open` | `() => void` | Opens the panel.
`close` | `() => void` | Closes the panel.
`cancelClose` | `() => void` | Cancels an in-flight `beforeClose` exit, e.g. if a gesture reopens it mid-close.
`expandAll` | `() => void` | Expands every node that has children.
`collapseAll` | `() => void` | Collapses every expanded node.
`expandNode` | `(value: string \| number) => void` | Expands a single node by value.
`collapseNode` | `(value: string \| number) => void` | Collapses a single node by value.
`findNode` | `(value: string \| number) => T \| undefined` | Depth-first search for a node by value, scoped to this instance's own `items`.
`findParent` | `(value: string \| number) => T \| null` | Depth-first search for a node's parent by the child's value, scoped to this instance's own `items`.
`removeNode` | `(value: string \| number) => boolean` | Removes a node by value (mutates `items` in place). Returns whether a match was found and removed.

