---
name: vael-form
description: Use whenever writing, editing, reviewing or debugging a Vue form that uses vael-form ("vael-form", "vael-form/validator" or "vael-form/nuxt" imports, `useForm` from 'vael-form', or vael-form in package.json), and when moving a Vue form from vee-validate to vael-form. Covers setup (VDOM and Vapor, formPlugin, the Nuxt module, vue-i18n), `useForm` with Zod, Valibot, ArkType or any Standard Schema (or plain `validate` functions), binding `model`, `attrs` and `errors` to plain inputs, vael-ui's Field/Input/Select and other component libraries, when errors show (`showErrors`), async `rules`, lists, steps and unions, submitting with server errors, FormData and TanStack Query, translated messages under `formKit`, SSR, the accessibility contract, and testing. Check it before guessing an option or return name, hand-rolling error timing, focus or ARIA wiring, or carrying a vee-validate pattern over.
license: MIT
metadata:
  author: Mini-Sylar
  version: 1.0.0
---

# vael-form `vael-form@0.0.3`

Vue-first forms on [Standard Schema](https://standardschema.dev): Zod, Valibot, ArkType, Yup, Joi, Effect… One composable (`useForm`), no components, no directives, no dependency on vael-ui. Works with plain inputs and any component library, in VDOM and Vapor. Peer: `vue ^3.5.0 || ^3.6.0-0`.

**References** (read from the library's types; if this page disagrees with one, the reference is right):
[API](./references/api.md) · [Setup, Nuxt, SSR](./references/guides/setup.md) · [Binding inputs & a11y](./references/guides/binding.md) · [Validation & rules](./references/guides/validation.md) · [Errors & i18n](./references/guides/errors-and-i18n.md) · [Submitting](./references/guides/submitting.md) · [Lists, steps, unions](./references/guides/lists-and-steps.md) · [Loading data, watch, dialogs, stores](./references/guides/data.md) · [Testing](./references/guides/testing.md) · [From vee-validate](./references/guides/from-vee-validate.md)

## How to work with this skill

1. **Open the guide for the task before writing.** Every option, return and field member is in [api.md](./references/api.md). Don't guess a name from vee-validate or React Hook Form: `defineField`, `useFieldArray`, `meta`, `resetForm`, `setFieldError`, `<ErrorMessage>` don't exist here.
2. **Walk the build sequence below in order.** Schema → form → bind each input → decide when errors show → submit → test. Most bugs come from skipping `attrs` on an input or re-implementing what the form already does (focus, ARIA, touched, debounce).
3. **Make the call, write the code.** Don't present option menus. State in one line what you chose when it wasn't obvious.

## Hard rules

1. **Destructure `model`, then `v-model` the ref.** `const { email, address } = model` and `v-model="email"`, `v-model="address.street"`. `v-model="model.email"` in a template doesn't unwrap (Vue unwraps top-level refs only). In the script it's `email.value`.
2. **Every input gets `v-bind="attrs.x"`.** It carries `id`, `name`, `aria-invalid`, `aria-describedby`, the schema's limits and `onFocusout`, which marks the field as left. Without it the default `'edited'` mode never shows the error and submit can't focus the field.
3. **The form is `<form novalidate @submit="handleSubmit">`.** No `.prevent` (it prevents for you). Don't put `required` on a control: that starts the browser's own validation. The schema is the truth; `attrs` adds `aria-required` from it, and vael-ui `Field`'s `required` is only the marker plus ARIA.
4. **Give each error a home with its id.** Plain inputs: `<p :id="attrs.x.errorId">{{ errors.x }}</p>`, kept in the page with a `min-height` so it doesn't shift the layout. With vael-ui: `<Field :error="errors.x">` renders it and merges `aria-describedby`; don't add a second `<p>`.
5. **No `role="alert"` or `aria-live` on field errors.** A failed submit focuses the first invalid field, which announces it with its description. (vael-ui 0.5.5's `Field` dropped `role="alert"` for this reason.)
6. **A group of radios or checkboxes is one field.** `attrs.x` goes on the `<fieldset>` or the `RadioGroup`, `v-model` on each option. The field counts as left when focus leaves the group.
7. **Lists live on `field('members')`.** `v-for="item in list.items" :key="item.key"`, bind `item.field.name.value` / `.attrs` / `.error`. Never key rows by index.
8. **Async checks go in `rules`, sync checks in the schema.** A rule gets `{ signal }`, is debounced, aborted when stale, skipped while the schema already fails at that path, and sets `pending`. Don't write an async `refine` that calls an API.
9. **Server errors go back with `form.setErrors({ email: '…' })`.** They show at once, clear when that field changes, and clear on the next submit. Don't keep them in your own ref.
10. **`handleSubmit(fn)` is made in the script.** `const save = handleSubmit(fn)` then `@submit="save"`. An inline `@submit="handleSubmit(fn)"` only creates the handler.
11. **Call `form.watch(path, …)` through the form** (or rename it): destructured `watch` shadows Vue's. `email.watch` / `form.watch` hear user edits and `setValue` only, not `reset()`, `setValues()` or loaded defaults.
12. **Messages come from your catalog, not an error map.** Install `formPlugin` with your i18n once and put the keys under `formKit`; don't write a Zod error map or translate inside the schema. One custom message: `.min(8, 'Pick a longer name')`, or `msg('errors.key')` to translate it.
13. **Loaded data is `defaultValues: dataRef`.** No `v-if` until it loads, no `watch` + `setValues`. In a dialog that stays mounted, `reset(record)` when you open it.
14. **Outside a component, give the form an `id`** (needed for stable ids in SSR) and, in a bare module with no app, pass `i18n`.

## The build sequence

### 1. Set up

```sh
pnpm add vael-form zod   # or valibot, arktype, yup, joi, effect…
```

```ts
// main.ts — only needed for translated messages or an app-wide showErrors
import { formPlugin } from 'vael-form'
app.use(i18n) // vue-i18n, either mode
app.use(formPlugin, { i18n: i18n.global, showErrors: 'edited' })
```

| Need | Do |
| --- | --- |
| Vapor | Nothing changes: same import, same API, in `<script setup vapor>` and Vapor-in-VDOM |
| Nuxt | `modules: ['@nuxtjs/i18n', 'vael-form/nuxt']`, options under `vaelForm` (`showErrors`, `keyPrefix`, `i18n`, `autoImports`). Auto-imports `useForm`, `useFormContext`, `useField`, `msg` |
| SSR | Ids come from `useId()`; the server render shows no errors. Forms made outside a component need `id` |
| One subtree's config | `provideFormConfig(computed(() => ({ i18n })))` |

Details: [setup.md](./references/guides/setup.md).

### 2. Write the schema

One schema holds every rule and always runs on every change; cross-field rules (`refine` with `path`) need nothing extra. Transforms live in the schema: the field keeps what the user typed, `onSubmit` gets the output (`z.coerce.number()` arrives as a number). Zod 4/Mini/3, Valibot and ArkType map to your catalog and give `attrs` their limits; Yup, Joi (`.prefs({ abortEarly: false })`) and Effect (`Schema.toStandardSchemaV1`) show their own text. No schema library: `validate: { email: [required, email] }`. [validation.md](./references/guides/validation.md).

### 3. Make the form

```vue
<script setup lang="ts">
import { z } from 'zod'
import { useForm } from 'vael-form'

const { model, errors, attrs, handleSubmit, submitting } = useForm({
  schema: z.object({ email: z.email(), password: z.string().min(8) }),
  defaultValues: { email: '', password: '' },
  onSubmit: async (values, { form }) => {
    try { await api.signUp(values) }
    catch { form.setErrors({ email: 'That email is already registered' }) }
  },
})
const { email, password } = model
</script>
```

State is refs you can destructure: `valid`, `dirty`, `touched`, `pending`, `submitting`, `submitCount`, `values`, `errorList`, `allErrors`. A group's own error, or one no field owns, is `field('members').error` / `field('').error`.

### 4. Bind each input

| Input | Binding |
| --- | --- |
| Plain text, email, textarea, date, select | `<input v-model="email" v-bind="attrs.email" />` + `<p :id="attrs.email.errorId">{{ errors.email }}</p>` |
| Number | `v-model.number`, or `z.coerce.number()` in the schema |
| One checkbox | `v-model` on a `boolean` field |
| Checkbox group / radios | list (or enum) field; `v-model` + `:value` on each, `attrs` on the `<fieldset>`; radios share a `name` |
| vael-ui | `<Field label="Email" :error="errors.email"><Input v-model="email" v-bind="attrs.email" /></Field>`. Same for `Select`, `InputNumber`, `Textarea`, `Checkbox`, `Switch`; `RadioGroup` takes the `attrs`. The Field's id wins so its label still names the control |
| A library whose passed `id` replaces its own | `:name="attrs.email.name" @focusout="attrs.email.onFocusout"` only |
| A reusable input | prop `field: Field<string>` from `form.field('email')`, or `useField(() => props.name)` |
| A child component / step | `useFormContext<typeof schema>()` |

[binding.md](./references/guides/binding.md) has the accessibility contract.

### 5. Decide when errors show

The schema always runs; `showErrors` only gates visibility. `'edited'` (default: changed and left), `'blur'`, `'input'`, `'submit'`, or per path: `{ default: 'edited', username: 'input' }`. Every mode shows everything after a submit. Autofill and a click on a button that the error moved are handled for you. [errors-and-i18n.md](./references/guides/errors-and-i18n.md).

### 6. Submit

`handleSubmit` prevents the default, validates (waiting for rules), then either calls `onSubmit(values, { form, event, element, submitter, formData })` or focuses the first invalid field and calls `onInvalid`. A second submit while one runs is ignored. Called from code (`await form.handleSubmit()`) it focuses too. Two buttons: `name`/`value` and read `submitter`. Multi-step: `await validate('account')`. TanStack Query: `onSubmit: (values) => mutation.mutateAsync(values)`. [submitting.md](./references/guides/submitting.md).

### 7. Test it

`await form.validate()` / `await form.handleSubmit()` return whether it was valid. In a DOM test, dispatch `input` then a bubbling `focusout` to mark a field left, then flush a couple of ticks. [testing.md](./references/guides/testing.md).

## Never ship

| Never | Instead |
| --- | --- |
| `v-model="model.email"` / `v-model="form.model.email"` in a template | destructure: `const { email } = model` |
| An input without `v-bind="attrs.x"` | always spread it (or `name` + `onFocusout` for a library that drops its label) |
| `@submit.prevent="handleSubmit"`, `required` on controls | `<form novalidate @submit="handleSubmit">`, rules in the schema |
| `role="alert"` on field errors, hand-written `aria-invalid` | `attrs` + focus-first-invalid |
| `:key="index"` on list rows | `:key="item.key"` |
| An async `refine` calling an API | `rules` with `signal` and `debounce` |
| A `serverError` ref next to the form | `form.setErrors` |
| `watch(() => data.value, (d) => setValues(d))` | `defaultValues: data` |
| A Zod error map, `t()` inside the schema | `formKit` keys in your catalog, `msg()` |
| `<Form>`, `<ErrorMessage>`, `defineField`, `useFieldArray` | they don't exist: [From vee-validate](./references/guides/from-vee-validate.md) |
| A name from memory | [api.md](./references/api.md) |

## Output

Write the code. Then, in a few lines: the binding you used for any non-text control (one line), anything you decided that the request left open (error timing, where server errors land), and what to check in the browser: tab through an empty form (no errors), submit it (first invalid field focused), and a screen reader reading the error with the field.
