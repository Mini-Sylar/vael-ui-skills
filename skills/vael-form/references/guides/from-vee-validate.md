# Moving a form from vee-validate

vael-form doesn't ship a migration guide. This mapping pairs vee-validate 4's documented API with vael-form's own types ([api.md](../api.md)); every vael-form name below is real. Migrate one form at a time: the two libraries don't share state, so they can coexist during the move.

## The shape changes

- **One composable, no components.** No `<Form>`, `<Field>`, `<ErrorMessage>`, no `defineField`, no `useFieldArray`. Templates use `v-model` on refs from `model`, `v-bind="attrs.x"`, and `errors.x`.
- **No schema adapter.** Pass the Zod/Valibot/ArkType schema itself (Standard Schema); drop `toTypedSchema` and `@vee-validate/zod`.
- **Validation always runs; only visibility is configured.** `validateOnBlur` / `validateOnChange` / `validateOnInput` / `validateOnModelUpdate` become one `showErrors` mode.
- **Messages come from your i18n catalog** under `formKit`, not a Zod error map, `configure({ generateMessage })` or `@vee-validate/i18n`.
- **ARIA and focus are built in.** Drop hand-written `aria-invalid`, `aria-describedby`, and "scroll to first error" code.

## Mapping

| vee-validate | vael-form |
| --- | --- |
| `useForm({ validationSchema: toTypedSchema(schema), initialValues })` | `useForm({ schema, defaultValues })` |
| `const [email, emailAttrs] = defineField('email')` | `const { email } = model` and `v-bind="attrs.email"` |
| `useField('email')` (in the form's component) | `form.field('email')` (`.value`, `.error`, `.attrs`, `.touched`…) |
| `useField(name)` in a reusable input (injected form) | `useField(() => props.name)`, or a `field: Field<string>` prop |
| `errors.value.email`, `errors.value['address.street']` | `errors.email`, `errors.address.street` (a tree, no `.value`) |
| `errorBag` | `field('x').errors`, `errorList`, `allErrors` |
| `values` | `values` (read-only; write via `model`, `setValue`, `setValues`) |
| `meta.value.valid / dirty / touched` | `valid`, `dirty`, `touched` refs |
| `isSubmitting`, `submitCount`, `isValidating` | `submitting`, `submitCount`, `pending` |
| `handleSubmit((values) => …)` | same: `handleSubmit((values, ctx) => …)` returns the handler; or `onSubmit` option + `@submit="handleSubmit"` |
| `handleSubmit(onSuccess, onInvalidSubmit)` | `onSubmit` + `onInvalid` options (or `handleSubmit(fn)` + `onInvalid`) |
| `@submit="onSubmit"` on `<form>` | `<form novalidate @submit="handleSubmit">` |
| `setFieldValue(path, v)` | `setValue(path, v)` (counts as a user edit) |
| `setValues(v)` | `setValues(v)` (replaces all; not reported to `watch`) |
| `setFieldError(path, msg)`, `setErrors({...})` | `setErrors({ [path]: msg })`; cleared when that field changes or on the next submit |
| `resetForm()`, `resetForm({ values })` | `reset()`, `reset(values)` (values become the new defaults) |
| `resetField('x')` | `field('x').reset()` |
| `validate()`, `validateField('x')` | `validate()`, `validate('x')` or `field('x').validate()` |
| `setFieldTouched('x', true/false)` | `field('x').touch()` / `field('x').untouch()` |
| `useFieldArray('members')` → `fields`, `push`, `remove`, `insert`, `swap`, `move`, `replace` | `field('members')` → `items` (`key`, `index`, `field`), same operations, plus `clear()` |
| `useFieldArray` `update(i, v)`, `prepend(v)` | `setValue('members.i', v)`, `insert(0, v)` |
| `entry.key` / `field.value` in `fields` | `item.key`; `item.field.name.value` |
| `configure({ validateOnBlur: true, validateOnChange: false… })` | `showErrors: 'blur'` (or `'edited'` default, `'input'`, `'submit'`, per path) |
| Zod error map / `generateMessage` / `@vee-validate/i18n` locale | `app.use(formPlugin, { i18n: i18n.global })` + a `formKit` block in your locale files |
| `defineRule('required', …)` global rules | plain functions in `validate: { email: [required, email] }`, or `validator()` from `vael-form/validator` |
| Async `.refine` hitting an API | `rules: { username: async (v, { signal }) => … }` with `debounce` |
| `useIsSubmitting()`, `useFormValues()`, `useFormErrors()` in a child | `useFormContext<typeof schema>()` and its refs |
| `keepValuesOnUnmount` | the default: values live in the form, nothing registers or unregisters |
| `initialValues` that load later + `resetForm` in a `watch` | `defaultValues: dataRef` (the user's edits win) |
| `<ErrorMessage name="email" />` | `<p :id="attrs.email.errorId">{{ errors.email }}</p>`, or vael-ui `<Field :error="errors.email">` |

## Steps for one form

1. Swap the import and options: `useForm({ schema, defaultValues, onSubmit })`. Remove `toTypedSchema`.
2. Replace each `defineField` pair: destructure `model`, put `v-bind="attrs.x"` on the control. With vael-ui: `<Field :error="errors.x"><Input v-model="x" v-bind="attrs.x" /></Field>`.
3. `<form novalidate @submit="handleSubmit">`. Remove `.prevent`, manual focus/scroll-to-error code, and `required` on controls.
4. Move server-error handling into `onSubmit`'s `catch` with `form.setErrors(…)` (a TanStack Query `onError` that called `setErrors` keeps working with the same call). Drop any branch that checked whether a submit was invalid: invalid submits never reach `onSubmit`; use `onInvalid` if you need to know.
5. Delete the Zod error map and vee-validate i18n setup; install `formPlugin` with your i18n and copy the `formKit` keys into each locale (see [errors-and-i18n.md](./errors-and-i18n.md)).
6. Arrays: `useFieldArray` → `field('members')`, keys from `item.key`.
7. Check timing: vee-validate shows a field's error once that field has been validated (by default on blur and on change); vael-form's default `'edited'` waits for changed-and-left. Set `showErrors` if the product wants the old feel.
8. Remove `vee-validate`, `@vee-validate/zod`, `@vee-validate/i18n` once no form imports them.
