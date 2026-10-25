# useListbox

The headless engine behind `<Select>`.

The active option and keyboard engine behind `<Select>` and `<Combobox>`. Focus stays on the trigger or input, and the active option travels through `aria-activedescendant`. The composable only tracks an index: it renders nothing and reads no DOM. `<Select>` adds the popover, filtering, virtualization and the option markup. Use it to build a listbox of your own with the same keyboard behaviour.

```ts
import { useListbox } from 'vael-ui' // or 'vael-ui/vapor'
```

## Example

```vue
<script setup lang="ts">
import { shallowRef, useId } from 'vue'
import { useListbox } from 'vael-ui'

const fruits = [
  { label: 'Apple', value: 'apple' },
  { label: 'Banana', value: 'banana', disabled: true },
  { label: 'Cherry', value: 'cherry' },
]
const picked = shallowRef<string>()
const listboxId = useId()
const { activeIndex, activeId, onKeydown, point, optionId } = useListbox({
  items: fruits,
  getLabel: (fruit) => fruit.label,
  isDisabled: (fruit) => !!fruit.disabled,
  onSelect: (fruit) => (picked.value = fruit.value),
  listboxId,
})
</script>

<template>
  <div
    tabindex="0"
    role="combobox"
    aria-expanded="true"
    :aria-controls="listboxId"
    :aria-activedescendant="activeId"
    @keydown="onKeydown"
  >
    {{ picked ?? 'Pick a fruit' }}
  </div>
  <ul :id="listboxId" role="listbox">
    <li
      v-for="(fruit, index) in fruits"
      :id="optionId(index)"
      :key="fruit.value"
      role="option"
      :aria-selected="picked === fruit.value"
      :aria-disabled="fruit.disabled"
      :data-active="index === activeIndex || undefined"
      @pointermove="point(index)"
    >
      {{ fruit.label }}
    </li>
  </ul>
</template>
```

## Parameters

Name | Type | Description
--- | --- | ---
`items` | `MaybeRefOrGetter<readonly T[]>` | The visible options, in order. Pass the filtered list when you filter, as `<Combobox>` does.
`getLabel` | `(item: T) => string` | The text typeahead matches against.
`isDisabled` | `(item: T) => boolean` | Disabled options are skipped by the arrow keys and typeahead, and Enter ignores them.
`onSelect` | `(item: T, index: number) => void` | Required. Fires when Enter commits the active option.
`onActiveChange` | `(index: number) => void` | Fires before `activeIndex` changes. A virtualized list scrolls the row into the DOM here, so `aria-activedescendant` never points at a missing id.
`listboxId` | `string` | Required. The listbox element's id. Option ids derive from it.

## Returns

Name | Type | Description
--- | --- | ---
`activeIndex` | `Ref<number>` | The active option's index, or `-1` for none. Reset it to `-1` yourself when the list closes.
`activeId` | `ComputedRef<string \| undefined>` | The active option's id. Bind it as `aria-activedescendant` on the focused element.
`onKeydown` | `(event: KeyboardEvent) => void` | Bind it on the focused element. Up and Down move between enabled options and wrap; with no active option, Down starts at the first and Up at the last. Home and End jump to the ends. Enter selects. Printable keys run a typeahead that resets after 500 ms. Space isn't handled, so a text input keeps it.
`setActive` | `(index: number) => void` | Makes `index` active from code, such as when the list opens. Calls `onActiveChange` first, so the list can scroll to it.
`point` | `(index: number) => void` | Makes `index` active without calling `onActiveChange`. Use it for a pointer moving over the options: the option is already on screen, and scrolling would move the list out from under the pointer.
`optionId` | `(index: number) => string` | The id to put on the option at `index`: `${listboxId}-opt-${index}`.
