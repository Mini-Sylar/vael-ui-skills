# Lists, steps and unions

Nothing registers or unregisters: values live in the form, not in the inputs.

## Lists

A list's rows and operations live on its field:

```vue
<script setup lang="ts">
import { z } from 'zod'
import { useForm } from 'vael-form'
import { Button, Field, Input } from 'vael-ui'

const { errors, field, handleSubmit } = useForm({
  schema: z.object({
    members: z
      .array(z.object({ name: z.string().min(2), email: z.email() }))
      .min(1, 'Add at least one member'),
  }),
  defaultValues: { members: [{ name: '', email: '' }] },
  onSubmit: (values) => api.invite(values.members),
})
const members = field('members')
</script>

<template>
  <form novalidate @submit="handleSubmit">
    <ul>
      <li v-for="item in members.items" :key="item.key">
        <Field label="Name" :error="item.field.name.error">
          <Input v-model="item.field.name.value" v-bind="item.field.name.attrs" />
        </Field>
        <Field label="Email" :error="item.field.email.error">
          <Input v-model="item.field.email.value" v-bind="item.field.email.attrs" type="email" />
        </Field>
        <Button variant="ghost" :aria-label="`Remove member ${item.index + 1}`" @click="members.remove(item.index)">×</Button>
      </li>
    </ul>
    <p v-if="errors.members">{{ errors.members }}</p>
    <Button variant="outline" @click="members.push({ name: '', email: '' })">Add member</Button>
    <Button type="submit">Send invites</Button>
  </form>
</template>
```

- `items`: `{ key, index, field }`. `item.key` is stable through edits, moves and removals: always the `v-for` key. Touched and edited state follow the row.
- Operations: `push`, `insert(index, value)`, `remove(index)`, `move(from, to)`, `swap(a, b)`, `replace(values)`, `clear()`. To change one row: `setValue('members.2', row)`; to prepend: `insert(0, row)`.
- The list's own error (`.min(1, …)`) is `errors.members` / `members.error`.
- `errors` and `attrs` treat a list as one leaf: `attrs.members[0]` doesn't exist. Per-row wiring is `item.field.name.attrs` (or `field(\`members.${i}.name\`).attrs`). The model's `v-model="row.name"` in a `v-for` over `model.members` writes values, but still take attrs and errors from the row's field.
- Rules and `showErrors` paths take `*` for any index: `'members.*.email'`.
- A row added after a submit stays quiet until it's left, or until the next submit.
- A list of checkboxes bound to one array field is not this: that's one field (`v-model="tags"` on each checkbox, `attrs.tags` on the fieldset).

## Steps

A step is a group in the same schema; one form holds all of them.

```ts
const { model, errors, attrs, validate, handleSubmit } = useForm({
  schema: z.object({
    account: z.object({ email: z.email(), password: z.string().min(8) }),
    profile: z.object({ name: z.string().min(2), role: z.enum(['designer', 'engineer']) }),
  }),
  onSubmit: (values) => api.signUp(values),
})
const { account, profile } = model
const steps = ['account', 'profile'] as const
const step = ref(0)
const last = computed(() => step.value === steps.length - 1)

async function next() {
  if (await validate(steps[step.value]!)) step.value++
}
```

```vue
<form novalidate @submit.prevent="last ? handleSubmit($event) : next()">
  <template v-if="step === 0">
    <Field label="Email" :error="errors.account.email">
      <Input v-model="account.email" v-bind="attrs.account.email" />
    </Field>
  </template>
  <template v-else>…</template>
  <Button v-if="step > 0" @click="step--">Back</Button>
  <Button type="submit">{{ last ? 'Create account' : 'Next' }}</Button>
</form>
```

- `validate(group)` checks one group and shows only its errors. Since 0.0.3 this works when the form has no `defaultValues` for that group (a required group the data doesn't have yet is checked as an empty object, for Zod, Valibot, ArkType).
- A step not on the page keeps what was typed. The final submit checks every step.
- `field('profile').valid` tells a step indicator which steps still have problems.
- `errorList` holds only what the user has earned; `allErrors` holds everything, including unreached steps.
- Which step is current is your own `ref`. A step in its own component reads the form with `useFormContext<typeof schema>()`.
- `@submit.prevent` is right here because the handler isn't `handleSubmit` itself; `handleSubmit($event)` still prevents and focuses.

## Unions

A discriminated union has the fields of every branch in `model`, `errors` and `attrs`; the schema validates the active one.

```ts
const schema = z.discriminatedUnion('method', [
  z.object({ method: z.literal('card'), number: z.string().min(12) }),
  z.object({ method: z.literal('bank'), iban: z.string().min(15) }),
])
const { model, attrs, errors } = useForm({ schema, defaultValues: { method: 'card' } })
const { method, number, iban } = model // both branches are there
```

`v-if="method === 'card'"` on the branch's inputs. Values typed in the other branch are kept (switching back restores them); `onSubmit` receives only the active branch.

## A schema that changes

```ts
const schema = ref(schemas.personal)
const form = useForm({ schema })
form.model.kind.watch(({ value }) => (schema.value = schemas[value])) // re-validates the same values
```
