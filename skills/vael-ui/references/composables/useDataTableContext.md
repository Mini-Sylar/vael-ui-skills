# useDataTableContext

The headless engine behind `<DataTable>`.

Reaches the nearest `<DataTable>`'s sort and selection state from a component rendered inside it, such as a custom cell. A slot function can't call a composable, so put the call in a small component and render that from a `<Column>`'s `#cell` or `#header` slot. `<DataTable>` provides the context with `provideDataTableContext()`, and `<Column>` registers itself through it. `useDataTableContext()` throws outside a DataTable.

```ts
import { useDataTableContext, provideDataTableContext } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<!-- StarCell.vue -->
<script setup lang="ts">
import { useDataTableContext } from 'vael-ui'

interface Person { id: number; name: string }
const props = defineProps<{ row: Person }>()
const table = useDataTableContext<Person>()
</script>

<template>
  <button :aria-pressed="table.isSelected(props.row)" @click="table.toggleSelect(props.row)">
    {{ table.isSelected(props.row) ? 'Starred' : 'Star' }}
  </button>
</template>

<!-- In the table -->
<DataTable :data="people" row-key="id" @update:selection="onSelection">
  <Column field="name" label="Name" />
  <Column field="id" label="">
    <template #cell="{ row }"><StarCell :row="row" /></template>
  </Column>
</DataTable>
```

## Parameters

Name | Type | Description
--- | --- | ---
`provideDataTableContext(ctx)` | `(ctx: DataTableContext<T>) => void` | What `<DataTable>` calls in its setup. Call it yourself only to render a cell component outside a table, in a test or a story, with a stub context.

## Returns

Name | Type | Description
--- | --- | ---
`sort` | `Ref<{ field: keyof T \| null; dir: 'asc' \| 'desc' \| null }>` | The table's `v-model:sort`. Writing it sorts the table.
`toggleSort` | `(field: keyof T) => void` | Cycles a column the way its header button does: a new field sorts ascending, then descending, then unsorted.
`selected` | `Ref<Set<string \| number>>` | The keys of the selected rows. Read it, and change the selection with `toggleSelect` so the table emits `update:selection`.
`toggleSelect` | `(row: T) => void` | Selects or deselects `row` and emits `update:selection` with the selected rows. With `single`, it replaces the selection.
`isSelected` | `(row: T) => boolean` | Whether `row` is selected.
`getRowKey` | `(row: T) => string \| number` | The row's key, from the table's `row-key`.
`registerColumn / unregisterColumn` | `(col: RegisteredColumn<T>) => void` | `<Column>` uses these to join and leave the table. You don't need them.
