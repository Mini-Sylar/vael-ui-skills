# vael-form API (0.0.3)

Read from `packages/vael-form/src/types.ts`, `index.ts`, `use-form.ts`, `config.ts`, `context.ts`, `i18n.ts`, `validator.ts` and `nuxt.ts`.

**Contents:** [Entry points](#entry-points) · [useForm](#useform) · [Options](#options) · [Returns](#returns-form) · [Field](#field) · [ArrayField](#arrayfield) · [FieldAttrs](#fieldattrs) · [Submit context](#submit-context) · [Rules](#rules) · [validate map and validator()](#validate-map-and-validator) · [formPlugin, provideFormConfig](#formplugin-and-provideformconfig) · [useFormContext, useField](#useformcontext-and-usefield) · [msg and Register](#msg-and-register) · [Nuxt module](#nuxt-module) · [Exported types](#exported-types) · [Ids](#ids) · [Versions](#versions)

## Entry points

| Import | What |
| --- | --- |
| `vael-form` | `useForm`, `useFormContext`, `useField`, `formPlugin`, `provideFormConfig`, `msg`, and the types below |
| `vael-form/validator` | `validator(map)`: a `validate` map as a Standard Schema |
| `vael-form/nuxt` | the Nuxt module (default export), config key `vaelForm` |
| `vael-form/runtime/plugin` | the module's Nuxt plugin (the module adds it; don't import it yourself) |

Dependencies: `@standard-schema/spec`. Peers: `vue ^3.5.0 || ^3.6.0-0`, optional `@nuxt/kit ^3.15.0 || ^4.0.0`. Lazy chunks load only when used: `rules`, the catalog readers (Zod/Valibot/ArkType issue → `formKit` key), the schema's limits for `attrs`, and the plain validator.

## useForm

```ts
// With a schema library (or any Standard Schema)
function useForm<S extends StandardSchemaV1>(options: UseFormOptions<S>): Form<S>
// With plain functions: types come from defaultValues, or name them: useForm<Values>({...})
function useForm<T extends object = Record<string, unknown>>(options: PlainFormOptions<T>): Form<StandardSchemaV1<T, T>>
```

Called during a component's setup it uses `useId()` for ids and `provide`s itself to children (`useFormContext`). Called elsewhere (a Pinia store, a module, a test inside `effectScope`) it works the same, but ids come from a counter unless you pass `id`. The app's `FormConfig` reaches a form whenever there is an injection context (Pinia stores have one). The returned form is `markRaw`, so Vue never wraps it.

## Options

`UseFormOptions<S>`:

| Option | Type | Notes |
| --- | --- | --- |
| `schema` | `S \| Ref<S>` | Required (or `validate`). Any Standard Schema. A ref swaps it at runtime; the same values are validated again. Its input type types the fields; its output types `onSubmit` |
| `defaultValues` | `MaybeRefOrGetter<DeepPartial<InferInput<S>> \| undefined>` | Starting values. A ref or getter that changes resets the form **unless the user has edited it** (dirty). May be `undefined` while loading. With Zod/Valibot/ArkType nested groups exist before the data does; with other libraries give them a default (`address: {}`) |
| `onSubmit` | `(values: InferOutput<S>, context: SubmittedContext<S>) => unknown` | Called by `handleSubmit` with the schema's output once valid. Awaited; `submitting` is true meanwhile. A throw rejects `handleSubmit` |
| `onInvalid` | `(errors: readonly FormError[], context: SubmitContext<S>) => unknown` | Called by `handleSubmit` when invalid, after focusing the first invalid field |
| `rules` | `Rules<InferInput<S>>` | Extra (usually async) checks by path, `*` for any list index. See [Rules](#rules) |
| `debounce` | `number` | ms a rule waits after the last change. Default `0` |
| `showErrors` | `ShowErrorsFor<InferInput<S>>` | `'edited'` (default) \| `'blur'` \| `'input'` \| `'submit'`, or `{ default?: mode, [path]: mode }` (nearest path wins). Replaces the app's value; not merged |
| `labels` | `{ [path]?: string }` | Text or an i18n key used as `{field}` in messages. Default: the name made readable (`firstName` → "First name") |
| `i18n` | `FormI18n` | Overrides the app's i18n for this form |
| `id` | `string` | Prefix for input ids. Give one outside a component when you server-render |

`PlainFormOptions<T>` = the same minus `schema`, plus:

| Option | Type | Notes |
| --- | --- | --- |
| `validate` | `ValidateMap<T>` | Required. Paths (`*` = list index, `''` = the form) → a rule or an array of rules (first failure wins). A rule may also be a schema for that one value |
| `defaultValues` | `MaybeRefOrGetter<DeepPartial<T> \| T \| undefined>` | Also gives the form its type |

## Returns (`Form`)

`Form<S>` is `Raw<FormShape<S>>`. Everything below is on it. Refs can be destructured and used in templates without `.value`.

| Member | Type | Notes |
| --- | --- | --- |
| `model` | `FormModel<I>` | A `ModelRef` per **top-level** field. Groups are writable objects (`address.street = 'x'`), lists arrays (`v-model="row.name"` in a `v-for`). Every write is a `setValue`. Each ref has `.watch(handler, options?)` (script only). Any name when the schema has no types |
| `errors` | `FormErrors<I>` | Tree of `string \| undefined`: `errors.email`, `errors.address.street`. A list is one leaf (`errors.members`, its own error) |
| `attrs` | `FormAttrs<I>` | Tree of [`FieldAttrs`](#fieldattrs): `attrs.email`, `attrs.address.street`. A list or group-of-options is one leaf |
| `field(path)` | `<P extends Path<I> \| ''>(path: P) => FieldFor<PathValue<I, P>>` | One [`Field`](#field) (or [`ArrayField`](#arrayfield) for a list). `''` is the form: `field('').error` is an error no field owns. Paths complete from the schema |
| `watch(path, handler, options?)` | `(change: { value, oldValue }) => unknown`; options `{ debounce?, immediate? }`; returns `stop` | Runs on user edits and `setValue`, not on `reset()`, `setValues()` or loaded defaults. Batched per tick; its own writes to that path don't re-run it. Stops with the component |
| `values` | `Readonly<Ref<DeepPartial<I>>>` | All values, read-only. Write through `model`, `setValue`, `setValues` |
| `setValue(path, value)` | typed by path | Counts as a user edit (dirty, `watch`, error timing); clears that path's server errors |
| `setValues(values)` | `DeepPartial<I>` | Replaces every value; not reported to `watch`; clears server errors |
| `errorList` | `Readonly<Ref<readonly FormError[]>>` | The errors being shown, in order (for a summary) |
| `allErrors` | `Readonly<Ref<readonly FormError[]>>` | Every error, shown or not |
| `valid` | `Readonly<Ref<boolean>>` | No errors at all, shown or not |
| `dirty` | `Readonly<Ref<boolean>>` | Values differ from defaults |
| `touched` | `Readonly<Ref<boolean>>` | Any field has been left |
| `pending` | `Readonly<Ref<boolean>>` | An async schema or a rule is waiting or running |
| `submitting` | `Readonly<Ref<boolean>>` | True while `handleSubmit` validates and `onSubmit` runs |
| `submitCount` | `Readonly<Ref<number>>` | Reset to 0 by `reset()` |
| `validate(path?)` | `(path?: Path<I>) => Promise<boolean>` | Validates (schema, then rules), shows errors under `path` (or all), resolves to whether that part is valid. Steps use this |
| `handleSubmit(event?)` | `(event?: Event) => Promise<boolean>` | The `@submit` handler. Resolves `false` when invalid or already submitting |
| `handleSubmit(fn)` | `(fn: (values, context: SubmittedContext<S>) => unknown) => (event?: Event) => Promise<boolean>` | Returns a handler that calls `fn` instead of `onSubmit`. Make it in the script |
| `reset(values?)` | `(values?: DeepPartial<I>) => void` | Back to the defaults, or to `values`, which become the new defaults. Clears touched, edited, `submitCount`, server errors and rule results |
| `setErrors(errors)` | `ServerErrors<I>`: `{ [path]?: string \| string[] }` or `{ path?, message }[]` | Always shown (not gated by `showErrors`). Appended to existing server errors. A field's next change clears its own; a submit clears all |
| `clearErrors()` | `() => void` | Clears every error set with `setErrors`. Takes no path |

### What `handleSubmit` does, in order

1. `event.preventDefault()`. If already submitting, resolves `false`.
2. `submitting = true`, `submitCount++`, clears server errors, reveals every field.
3. Runs the schema, then waits for every rule.
4. Invalid: waits a tick, focuses the first `[aria-invalid="true"]` inside the submitting `<form>` (or, called from code with no event, the first invalid field on the page by its id, or by the error id in its `aria-describedby`), calls `onInvalid(shownErrors, context)`, resolves `false`.
5. Valid: awaits `onSubmit(output, context)` (or your `fn`), resolves `true`. A throw rejects. `submitting = false` in every case.

## Field

`Field<T>` (from `form.field(path)`, `item.field.x`, `useField`):

| Member | Type | Notes |
| --- | --- | --- |
| `path` | `string` | `'address.street'`, `'members.2.name'` |
| `id` | `string` | `${formId}-${path with . → -}` |
| `errorId` | `string` | `${id}-error` |
| `value` | `T \| undefined` | Read/write; works with `v-model="field.value"` |
| `error` | `string \| undefined` | First error to show (follows `showErrors`) |
| `errors` | `readonly string[]` | Every error to show |
| `allErrors` | `readonly FormError[]` | Every error, shown or not (live checklist before touch) |
| `touched` | `boolean` | Left at least once |
| `dirty` | `boolean` | Differs from its default |
| `valid` | `boolean` | No errors at this path or under it (use for step indicators: `field('profile').valid`) |
| `pending` | `boolean` | A rule here or below is waiting/running |
| `attrs` | `FieldAttrs` | What to spread on the input |
| `watch(handler, options?)` | returns `stop` | As `form.watch` for this path |
| `validate()` | `Promise<boolean>` | Validates the form, shows this field's errors |
| `touch()` | `void` | Marks left, immediately |
| `untouch()` | `void` | Hides its errors again until it's left once more |
| `reset(value?)` | `void` | Back to its default (or `value`), untouched; not a user edit |

## ArrayField

`ArrayField<T> extends Field<T[]>`, from `field('members')` on a list:

| Member | Notes |
| --- | --- |
| `items` | `readonly { key: string; index: number; field: FieldTree<T> }[]`. `key` is stable through edits and moves: use it as the `v-for` key. `item.field.name` is a `Field` (`.value`, `.attrs`, `.error`) |
| `push(value)` | append |
| `insert(index, value)` | clamps the index |
| `remove(index)` | |
| `move(from, to)` | touched/edited state follows the row |
| `swap(a, b)` | |
| `replace(values)` | new rows, not a user edit |
| `clear()` | removes every row |

No `update` or `prepend`: use `setValue('members.2', row)` and `insert(0, row)`.

## FieldAttrs

```ts
type FieldAttrs = {
  readonly id: string
  readonly name: string                       // the path: 'address.street', 'members.0.name'
  readonly 'aria-invalid': true | undefined   // only while an error is shown
  readonly 'aria-describedby': string | undefined // = errorId, only while an error is shown
  readonly onFocusout: (event?: FocusEvent) => void // marks the field as left
  readonly errorId: string                    // non-enumerable: not spread onto the input
  // From the schema (Zod, Valibot, ArkType), once its small chunk has loaded:
  readonly 'aria-required'?: true             // required string with a minimum, required number; not booleans or plain strings
  readonly minlength?: number
  readonly maxlength?: number
  readonly min?: number
  readonly max?: number
  readonly step?: number                      // from multipleOf
}
```

`onFocusout` ignores focus moving inside the same element (a fieldset, a component's own parts), ignores a field that still holds a browser-autofilled value, and when the field loses focus to a pointer press it waits for the release (so the click lands before the error shifts the layout). `field.touch()` is always immediate.

## Submit context

```ts
interface SubmitContext<S> {
  form: Form<S>                  // the same form, safe to use inside its own options
  event: Event | undefined
  element: HTMLFormElement | null // the <form>; event.currentTarget is already null by now
  submitter: HTMLElement | null  // the button that submitted: submitter.value for name/value buttons
}
interface SubmittedContext<S> extends SubmitContext<S> {
  readonly formData: FormData    // built on read, from the validated output
}
```

`formData`: nested names (`address.street`, `members.0.name`), `File`/`Blob` kept, `Date` → ISO string, booleans `'true'`/`'false'`, `null`/`undefined` skipped, plus the submitter's `name`/`value`. Values from steps not on screen are included.

## Rules

```ts
type Rules<I> = { [P in Path<I> | Path<I, [], '*'>]?: RuleInput<PathValue<I, P>, I> }
type RuleInput<V, I> = RuleCheck<V, I> | { run: RuleCheck<V, I>; on?: 'change' | 'blur'; debounce?: number }
type RuleCheck<V, I> = RuleFn<V, I> | StandardSchemaV1<V, unknown>   // a schema for this one value
type RuleFn<V, I> = (value: V, context: RuleContext<I>) => RuleResult | Promise<RuleResult>
interface RuleContext<I> { values: DeepPartial<I>; path: string; signal: AbortSignal }
type RuleResult = string | readonly string[] | boolean | null | undefined // message(s), or nothing/true when fine
```

A rule runs only when the schema has no error at its path and the value isn't the default one. `on: 'blur'` checks when the field is left; a submit runs it anyway. A newer change aborts the older call (`signal`) and only the newest answer counts. A rule that throws stops the submit and rethrows.

## validate map and validator()

```ts
type ValidateMap<T> = { [P in Path<T> | Path<T, [], '*'> | '']?: RuleList<PathValue<T, P>, T> }
type RuleList<V, I> = RuleCheck<V, I> | readonly RuleCheck<V, I>[]

import { validator } from 'vael-form/validator'
function validator<T>(rules: ValidateMap<T>): StandardSchemaV1<T, T>
```

Rules get `(value, { values, path, signal })`. An array runs in order; the first failure is the error. A rule returning a promise makes the whole validation async. The output is the values as they are (nothing stripped, trimmed or converted). `validator()` is a Standard Schema: `useForm({ schema: signup, defaultValues })`, or `signup['~standard'].validate(body)` on a server.

## formPlugin and provideFormConfig

```ts
const formPlugin: ObjectPlugin<[config?: FormConfig]>  // app.use(formPlugin, config)
function provideFormConfig(config: MaybeRefOrGetter<FormConfig>): void // in a component's setup

interface FormConfig {
  i18n?: FormI18n
  showErrors?: ShowErrorsFor
  keyPrefix?: string // default 'formKit'
}
interface FormI18n {
  t: (key: string, params?: MessageParams, options?: Record<string, unknown>) => string
  locale?: MaybeRefOrGetter<string> // errors re-resolve when it changes
}
```

vue-i18n's `i18n.global` fits in composition or legacy mode; so does `{ t, locale }` over any catalog. The injection key is `Symbol.for('vael-form:config')`, so two copies of the package still meet.

## useFormContext and useField

```ts
function useFormContext<S extends StandardSchemaV1 = StandardSchemaV1>(): Form<S>
// throws '[vael-form] useFormContext() needs a useForm() in a parent component'
function useField<T = unknown>(path: MaybeRefOrGetter<string>): FieldFor<T>
// a field of the nearest form; follows a ref/getter path: useField<string>(() => props.name)
```

Type the context with the schema: `useFormContext<typeof schema>()` (import the schema with `import type` if it lives in another file).

## msg and Register

```ts
function msg(key: MessageKey | readonly MessageKey[], params?: MessageParams, fallback?: string): string
```

Returns a tagged string (any schema library accepts it) that the form translates when the error is read, so a locale switch shows at once. A list of keys is tried in order. Use it in a schema (`refine(isFree, msg('errors.taken', { name }, 'Already taken'))`), a rule, or a `validate` function.

```ts
declare module 'vael-form' {
  interface Register { messageKeys: 'errors.taken' | 'errors.members' }
}
```

## Nuxt module

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@nuxtjs/i18n', 'vael-form/nuxt'],
  vaelForm: { showErrors: 'edited' },
})
```

| Option | Default | Notes |
| --- | --- | --- |
| `showErrors` | — | for every form |
| `keyPrefix` | `'formKit'` | |
| `i18n` | `true` | use the app's `$i18n` (`@nuxtjs/i18n`), read lazily so module order doesn't matter |
| `autoImports` | `true` | `useForm`, `useFormContext`, `useField`, `msg` |

## Exported types

`ArrayField`, `ArrayItem`, `DeepPartial`, `Field`, `FieldAttrs`, `FieldChange` (`{ value, oldValue }`), `FieldWatchOptions` (`{ debounce?, immediate? }`), `FieldFor`, `FieldTree`, `Form`, `FormConfig`, `FormAttrs`, `FormErrors`, `FormFields`, `FormModel`, `ModelFor`, `ModelRef` (`Ref<T> & { watch }`), `FormI18n`, `FormError` (`{ path, message }`), `InferInput`, `InferOutput`, `MessageKey`, `MessageParams`, `Path`, `PathValue`, `Register`, `RuleContext`, `RuleFn`, `RuleInput`, `RuleResult`, `RuleList`, `Rules`, `ServerErrors`, `ShowErrors`, `ShowErrorsFor`, `SubmitContext`, `SubmittedContext`, `PlainFormOptions`, `UseFormOptions`, `ValidateMap`, and `StandardSchemaV1` (re-exported).

Type notes: paths are checked to twelve levels (recursive schemas past that accept any suffix). A discriminated union at the top has the fields of every branch in `model`, `errors` and `attrs`. A tuple is one field with positions as paths (`range.0`). A nullable value is `Field<string | null>`. A schema with no types accepts any name.

## Ids

Input id `${formId}-${path.replaceAll('.', '-')}`, error id `${inputId}-error`. `formId` is `useId()` in a component, `options.id` if given, else a counter.

## Versions

- **0.0.1**: first release.
- **0.0.2**: `handleSubmit()` called from code focuses the first invalid field too (found by id, or by the error id in `aria-describedby` when a component library gives the input its own id).
- **0.0.3**: `validate('group')` shows a step's field errors when the form has no defaults for that group; an unchosen radio/cleared select shows `required` instead of `invalidValue`; `model` has every union branch's fields; `FieldAttrs` declares `aria-required`, `minlength`, `maxlength`, `min`, `max`, `step`.

Not in the library yet: devtools.
