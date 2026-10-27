# useTween

A number that eases to each new value of its source, from wherever it is: a cart total, a percentage, a value you draw yourself. Change the source mid-way and it turns toward the new value from where it shows, with no jump. It rides the app's spring unless you give it a `spring`, or a `duration` and `easing`. With `motion="false"` on a `ConfigProvider` around it, or under reduced motion, it jumps. For a value that should keep its speed through a change, such as one following a drag, use `useSpringValue`.

```ts
import { useTween } from 'vael-ui/motion' // either build
```

## Example

```vue
<script setup lang="ts">
import { shallowRef } from 'vue'
import { useTween } from 'vael-ui/motion'

const total = shallowRef(0)
// Eases to each new total from wherever it is.
const shown = useTween(total, { duration: 600, easing: 'out' })
const money = new Intl.NumberFormat('en', { style: 'currency', currency: 'USD' })
</script>

<template>
  <p>{{ money.format(shown) }}</p>
</template>
```

## Parameters

Name | Type | Description
--- | --- | ---
`source` | `MaybeRefOrGetter<number>` | The value to follow: a ref, a getter such as `() => props.total`, or a number.
`options` | `MaybeRefOrGetter<TweenOptions>` | Its timing: `spring` (a name or `{ damping, response }`), or `duration` in ms and `easing` (a token such as `'out'`, or any CSS easing), and `delay` in seconds. Unset, the app's spring.

## Returns

Name | Type | Description
--- | --- | ---
`value` | `Readonly<ShallowRef<number>>` | The number as it eases, every frame. Bind it in a template or read it in a computed.
