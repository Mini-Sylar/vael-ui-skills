# Submitting

```vue
<form novalidate @submit="handleSubmit">…</form>
```

`handleSubmit` prevents the default, ignores a second submit while one runs, clears server errors, validates (waiting for `rules`), and then:

- **valid**: awaits `onSubmit(values, context)` with the schema's **output** (`z.coerce.number()` arrives as a number). `submitting` is true until it settles. A throw rejects `handleSubmit` and `submitting` goes back to false.
- **invalid**: focuses the first invalid field and calls `onInvalid(errors, context)`.

It resolves to whether the form was valid.

## The context

`onSubmit(values, { form, event, element, submitter, formData })`:

- `form`: the same form, so the callback never names a variable that isn't defined yet (handy in a store).
- `element`: the `<form>` (`event.currentTarget` is already null after validation awaited).
- `submitter`: the button that submitted.
- `formData`: the values as multipart, built when read.

## Server errors

```ts
const { model, errors, attrs, handleSubmit, submitting } = useForm({
  schema: z.object({ email: z.email() }),
  defaultValues: { email: '' },
  onSubmit: async (values, { form }) => {
    try {
      await api.invite(values.email)
    } catch (error) {
      form.setErrors({ email: (error as Error).message })
    }
  },
})
```

- `setErrors({ path: message | message[] })` or `setErrors([{ path?, message }])` (no path = the form's own error, `field('').error`).
- Shown at once (not gated by `showErrors`), cleared when that field changes (`setValue` on it or under it), on `setValues`, `reset`, `clearErrors()`, and at the next submit.
- Map an API's field errors straight in: `form.setErrors(response.errors)` when it's `{ path: message }`.

## Two buttons, one form

```vue
<button name="action" value="draft">Save draft</button>
<button name="action" value="publish">Publish</button>
```

```ts
onSubmit: (values, { submitter }) => api.save(values, (submitter as HTMLButtonElement | null)?.value)
```

With a component library's button, read `submitter?.getAttribute('value')`.

## A handler of your own

`handleSubmit(fn)` returns a handler that calls `fn` (same values and context) instead of `onSubmit`. Make it in the script:

```ts
const save = handleSubmit(async (values, { formData }) => {
  await api.save(values)
})
// <form novalidate @submit="save">
```

An inline `@submit="handleSubmit(fn)"` would only create the handler, not run it.

## A submit button outside the form

Give the form an `id` and the button `form="that-id"` (a dialog footer):

```vue
<form id="edit-member" novalidate @submit="handleSubmit">…</form>
<Button type="submit" form="edit-member" :loading="submitting">Save</Button>
```

Or call it from code: `await form.handleSubmit()` validates, shows every error and focuses the first invalid field (by its id, or by the error id in its `aria-describedby` when a component library gives the input its own id).

## Steps

`await validate('account')` checks one group and shows only its errors; the final `handleSubmit` checks every step. See [lists-and-steps.md](./lists-and-steps.md).

## FormData

```ts
onSubmit: (values, { formData }) => fetch('/api/signup', { method: 'POST', body: formData })
```

Built from the values (the schema's output), not from the page: steps not on screen are in it, a checkbox is `'true'` not `'on'`. Nested names follow the inputs (`address.street`, `members.0.name`), `File`s stay files, dates become ISO strings, `null`/`undefined` are left out, and the pressed submit button's name and value are added, as a native submit does. For a file input set the field yourself: `@change="avatar = $event.target.files[0]"`.

## With TanStack Query

The form checks the values, a mutation sends them:

```ts
const mutation = useMutation({ mutationFn: (values: Values) => api.save(values) })
const form = useForm({
  schema,
  onSubmit: (values, { form }) =>
    mutation.mutateAsync(values).catch((error) => form.setErrors(toFieldErrors(error))),
})
// <form novalidate @submit="form.handleSubmit">, <Button type="submit" :loading="mutation.isPending.value">
```

To keep calling `mutate()` as before, write the pairing once:

```ts
export function useFormMutation<S extends StandardSchemaV1, T>(
  form: Form<S>,
  mutationFn: (values: InferOutput<S>) => Promise<T>,
) {
  const mutation = useMutation({ mutationFn })
  // Only valid values reach the mutation; its errors stay on the mutation, not thrown here.
  const submit = form.handleSubmit((values) => mutation.mutateAsync(values).catch(() => {}))
  return { ...mutation, mutate: () => void submit() }
}
```

A server's field errors go back with `form.setErrors` in the mutation's `onError`.

## After a save

`reset(saved)` makes the saved values the new defaults (nothing dirty). `onSubmit: (saved, { form }) => form.reset(saved)` is the whole "save and stay on the page" pattern.
