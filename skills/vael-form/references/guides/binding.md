# Binding inputs, and the accessibility contract

Per input there are three things, all looked up by the field's name: `v-model` writes the value, `attrs` wires the id, ARIA and blur, and `errors` is the message.

## The model

```ts
const { model, attrs, errors } = useForm({
  schema,
  defaultValues: { email: '', address: { street: '' }, tags: [] },
})
const { email, address, tags } = model // refs: destructure them
```

- `model` has a ref per **top-level** field. A group is a ref to an object you can write to (`address.street`), a list a ref to an array (`v-model="row.name"` in a `v-for` works).
- Destructure it. `model.address.street` in a template doesn't unwrap (Vue unwraps top-level refs only).
- In the script a ref is a ref: `email.value = 'x'`. Every write is a `setValue`: dirty, errors and `watch` behave as for a user edit.
- With Zod, Valibot or ArkType a group exists before the data has it (read from the schema). With another library give it a default (`address: {}`).

## Plain inputs

```vue
<form novalidate @submit="handleSubmit">
  <label :for="attrs.email.id">Email</label>
  <input v-model="email" v-bind="attrs.email" type="email" autocomplete="email" />
  <p :id="attrs.email.errorId" class="error">{{ errors.email }}</p>

  <input v-model="address.street" v-bind="attrs.address.street" />
  <p :id="attrs.address.street.errorId" class="error">{{ errors.address.street }}</p>
</form>
```

| Input | Binding |
| --- | --- |
| text, email, textarea, date, select | `<input v-model="x" v-bind="attrs.x" />` |
| number | `v-model.number="x"`, or `z.coerce.number()` in the schema |
| one checkbox | `v-model="x"` on a `boolean` field |
| checkbox group | a list field: `v-model="x"` on each checkbox with its own `:value` |
| radio group | `v-model="x"` on each radio with its own `:value`, a shared `name` so arrow keys move between them |
| file | set it yourself: `@change="avatar = $event.target.files[0]"` (script: `avatar.value = …`) |

A group of checkboxes or radios has one `id` and one error, so `attrs.x` goes on the wrapping `<fieldset>`. The field counts as left when focus leaves the group, not when it moves between its inputs.

```vue
<fieldset v-bind="attrs.tags">
  <legend>Tags</legend>
  <label v-for="tag in tagList" :key="tag">
    <input v-model="tags" type="checkbox" :value="tag" /> {{ tag }}
  </label>
</fieldset>
<p :id="attrs.tags.errorId" class="error">{{ errors.tags }}</p>
```

`v-model.trim` and `v-model.number` work as in any Vue form. Your own `@blur` / `@focusout` still runs next to the one in `attrs`.

## vael-ui

`attrs` works on vael-ui's controls because they keep their own wiring when attributes are passed in: inside a `Field`, the Field's `id` wins (so its label still names the control) and the passed `aria-describedby` is merged with the Field's error.

```vue
<script setup lang="ts">
import { z } from 'zod'
import { useForm } from 'vael-form'
import { Button, Checkbox, Field, Input, InputNumber, Radio, RadioGroup, Select, Switch } from 'vael-ui'

const { model, errors, attrs, handleSubmit, submitting } = useForm({
  schema: z.object({
    email: z.email(),
    role: z.enum(['designer', 'engineer']),
    plan: z.enum(['free', 'pro']),
    seats: z.number().int().min(1).max(50),
    topics: z.array(z.string()).min(1),
    terms: z.boolean().refine((v) => v, 'Accept the terms to continue'),
    notify: z.boolean(),
  }),
  defaultValues: { email: '', seats: 1, topics: [], terms: false, notify: false },
  onSubmit: (values) => api.save(values),
})
const { email, role, plan, seats, topics, terms, notify } = model
</script>

<template>
  <form novalidate @submit="handleSubmit">
    <Field label="Email" :error="errors.email">
      <Input v-model="email" v-bind="attrs.email" type="email" />
    </Field>
    <Field label="Role" :error="errors.role">
      <Select v-model="role" v-bind="attrs.role" :items="roles" placeholder="Choose…" />
    </Field>
    <Field label="Plan" :error="errors.plan">
      <RadioGroup v-model="plan" v-bind="attrs.plan">
        <Radio value="free" label="Free" />
        <Radio value="pro" label="Pro" />
      </RadioGroup>
    </Field>
    <Field label="Seats" :error="errors.seats">
      <InputNumber v-model="seats" v-bind="attrs.seats" />
    </Field>
    <Field label="Topics" :error="errors.topics">
      <fieldset v-bind="attrs.topics">
        <Checkbox v-for="t in topicList" :key="t" v-model="topics" :value="t" :label="t" />
      </fieldset>
    </Field>
    <Field :error="errors.terms">
      <Checkbox v-model="terms" v-bind="attrs.terms" label="I accept the terms" />
    </Field>
    <Field :error="errors.notify">
      <Switch v-model="notify" v-bind="attrs.notify" label="Email me" />
    </Field>
    <Button type="submit" :loading="submitting">Save</Button>
  </form>
</template>
```

- `Field :error` renders the message with its own id and links it; don't render a second `<p>`.
- vael-ui 0.5.5's `Field` error has no `role="alert"`: vael-form focuses the first invalid field on submit, which announces it with its description. Don't add one.
- `Field required` is the visual marker plus `aria-required` only. `required` placed on a control starts the browser's own validation: leave it off and let the schema decide (`attrs` already adds `aria-required` for values that can't be empty).
- An async rule's spinner: `<Input :loading="username.pending">` via `field('username').pending`, or the form's `pending`.
- `Button type="submit" :loading="submitting"`; a footer button outside the `<form>` uses `form="form-id"` (as in a `Dialog`'s `#footer`).
- Don't import `vael-ui`'s `Field` and vael-form's `Field` type under the same name: `import type { Field as FormField } from 'vael-form'`.

## Other component libraries

`attrs` works on a library's components when the library keeps its own wiring with passed attributes. A library that lets a passed `id` replace its own would lose its label link. There, spread only what it needs:

```vue
<TheirInput v-model="email" :name="attrs.email.name" @focusout="attrs.email.onFocusout" />
```

and render the error with its message prop. Submit still finds the field: called with a submit event it focuses the first `[aria-invalid="true"]` in the form; called from code it looks for the field's id, or an element whose `aria-describedby` contains the field's error id.

## Reusable inputs

A field is a typed object, so reuse is a prop:

```vue
<!-- TextField.vue -->
<script setup lang="ts">
import type { Field } from 'vael-form'
defineProps<{ field: Field<string>; label: string }>()
</script>
<template>
  <label :for="field.id">{{ label }}</label>
  <input v-model="field.value" v-bind="field.attrs" />
  <p :id="field.errorId" class="error">{{ field.error }}</p>
</template>
<!-- <TextField :field="form.field('email')" label="Email" /> -->
```

Binding by name to the nearest form instead: `const field = useField<string>(() => props.name)` (follows a changing `name`).

## Child components and steps

```ts
// AccountStep.vue, rendered inside the component that called useForm
import { useFormContext } from 'vael-form'
import type { schema } from './schema'
const { model, errors, attrs } = useFormContext<typeof schema>()
const { account } = model // then v-model="account.email" v-bind="attrs.account.email"
```

`useFormContext` throws when no parent component called `useForm`.

## The accessibility contract

What vael-form does, so you don't re-implement it:

| Concern | Done by |
| --- | --- |
| `id` / `name` | `attrs.x.id` (`${formId}-${path}`), `name` = the path |
| `aria-invalid="true"` | `attrs`, only while an error is **shown** |
| `aria-describedby` | `attrs`, pointing at `errorId`, only while an error is shown |
| `aria-required` | `attrs`, from the schema (Zod, Valibot, ArkType): a required string with a minimum, a required number; not a boolean or a plain string |
| `minlength`, `maxlength`, `min`, `max`, `step` | `attrs`, from the schema; `maxlength` stops typing at the limit |
| Focus on failed submit | `handleSubmit` focuses the first invalid field, with or without a submit event |
| Not scolding too early | `showErrors: 'edited'` (default): an error shows once the field is changed and left |
| Autofill | a field still holding an autofilled value isn't "left" by the browser's synthetic blur |
| Errors that move the button you're clicking | leaving a field by pointer press waits for the release, so the click lands first |
| Ids stable across SSR | `useId()` |

What you do:

- Label every control (`<label :for="attrs.x.id">`, or vael-ui `Field label`). A group gets a `<fieldset>` + `<legend>` (or `Field label` around a `RadioGroup`).
- Render the error element with `:id="attrs.x.errorId"` (plain inputs). Keep it in the page even when empty with a `min-height` so an error doesn't shift what's below; `aria-describedby` points at it only while invalid.
- `novalidate` on the `<form>`.
- An error summary for long forms from `errorList`, linking each message to `#${attrs[path].id}`; a list's own error (`errors.members`) can sit near the list.
- Status after success (`<p role="status">Saved</p>`) is yours to render.
