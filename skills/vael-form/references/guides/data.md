# Loading data, reacting to changes, dialogs

## Defaults that load later

Pass a ref (from `useFetch`, a query, or your own). The form is usable while it's `undefined`, takes the data when it arrives, and keeps what the user already typed.

```ts
const { data: user } = useQuery(…)
const form = useForm({ schema, defaultValues: user })
```

- Data arriving before an edit replaces the values and clears `dirty`. After an edit, the user's edit wins (`form.reset(user.value)` overwrites on purpose).
- Nested groups (`notify.email`) work while loading with Zod, Valibot or ArkType; with another library give them a default.
- Show a skeleton with the query's own `isLoading`. With Suspense, `defaultValues: await load()` is a plain value.
- After a save, `form.reset(saved)` makes the saved values the new defaults.
- Don't `watch(data, (d) => setValues(d))`: that bypasses the "user's edit wins" rule.

## Fields that follow others

`ref.watch` (on a `model` ref, in the script) and `form.watch(path, …)` run on a user edit or `setValue`. They don't run for `reset()`, `setValues()` or defaults that load, so loaded data with both a country and a state keeps both.

```ts
const form = useForm({ schema, defaultValues })
const { country, state, plan, username } = form.model

country.watch(() => (state.value = undefined))                // a dependent field
plan.watch(({ value }) => (schema.value = schemas[value]))     // swap the schema (a ref)
username.watch(({ value }) => track(value), { debounce: 300 })
form.watch('address.zip', ({ value }) => lookup(value))        // any path, a group or a list too
```

- Handler gets `{ value, oldValue }`. Several changes in one tick run it once. What it writes to its own field doesn't run it again; other watchers do run.
- Options: `debounce`, `immediate`. Returns `stop`; stops with the component.
- Call `form.watch` through the form, or rename it when destructuring: a bare `watch` shadows Vue's.
- Use Vue's `watch` for anything that isn't a form value, or when a reset or load *should* trigger it.

Three kinds of dependent values:

| Kind | Where |
| --- | --- |
| A rule across fields (confirm password) | the schema: `refine` with a `path` (nothing else needed; the whole schema runs on every change) |
| A value that follows another | `country.watch(...)` |
| Options that depend on a value | `const states = computed(() => statesOf[country.value])` |

## Dialogs

Put the form in its own component inside the dialog. A dialog that renders its content only while open (vael-ui's `Dialog` does, unless `forceMount`) makes a new form on every open, already filled in, with nothing to reset:

```vue
<Dialog v-model:open="open" title="Edit user"><EditUser :user="selected" @done="open = false" /></Dialog>
```

```ts
// EditUser.vue
const props = defineProps<{ user: User }>()
const form = useForm({ schema, defaultValues: props.user, onSubmit: save })
```

If the form stays mounted (a hidden dialog, the parent, a store), reset it where you open the dialog:

```ts
function openEdit(user: User) {
  form.reset(user) // new defaults, nothing dirty, nothing touched
  open.value = true
}
```

Why: `defaultValues` follows its source only while the form isn't dirty, so a shared dialog that shows record A, is cancelled with edits, then shows record B would keep A's edits. A fresh component, or `reset(record)` on open, avoids it.

## Stores and composables

See [setup.md](./setup.md#outside-a-component): a form works in a Pinia store (give it an `id` for SSR), the `form` in `onSubmit`'s context is the same form, and refs stay refs (`form.valid.value` in a script).
