# Validation: schemas, rules, plain functions

One schema is the truth and the whole schema runs on every change. `showErrors` only decides when an error becomes visible (see [errors-and-i18n.md](./errors-and-i18n.md)).

## Schema libraries

| Library | Errors map to your `formKit` catalog | `attrs` limits | Notes |
| --- | --- | --- | --- |
| Zod 4, Zod Mini, Zod 3 | yes | yes | |
| Valibot | yes | yes | |
| ArkType | yes | yes | |
| Yup, Joi, Effect, any other Standard Schema | no: their own text (use `msg()` for translated ones) | no | Joi: end with `.prefs({ abortEarly: false })` to report every field. Effect: `Schema.toStandardSchemaV1(schema)` |

```ts
z.object({ email: z.email(), age: z.number().min(18) })
v.object({ email: v.pipe(v.string(), v.email()), age: v.pipe(v.number(), v.minValue(18)) })
type({ email: 'string.email', age: 'number >= 18' })
```

No adapter (`toTypedSchema`, resolvers): pass the schema as is. The input type types `model`, `errors`, `attrs` and paths; the output type types `onSubmit`.

## Rules across fields

A cross-field rule goes in the schema with a `path`; the error lands on that field and clears when either side is fixed.

```ts
// Zod (runs even when another field is invalid, so both errors show at once)
z.object({ password: z.string().min(8), confirm: z.string() }).refine(
  (d) => d.password === d.confirm,
  { path: ['confirm'], message: 'Passwords do not match' },
)

// Valibot
v.pipe(
  v.object({ password: v.pipe(v.string(), v.minLength(8)), confirm: v.string() }),
  v.forward(
    v.partialCheck([['password'], ['confirm']], (i) => i.password === i.confirm, 'Passwords do not match'),
    ['confirm'],
  ),
)

// ArkType
type({ password: 'string >= 8', confirm: 'string' }).narrow(
  (data, ctx) =>
    data.password === data.confirm ||
    ctx.reject({ expected: 'identical to password', actual: '', path: ['confirm'], message: 'Passwords do not match' }),
)
```

- A field required only for some answers: `superRefine` (Zod) or a discriminated union (`z.discriminatedUnion`, `v.variant`) plus `v-if` on the input.
- An error no field owns (a `refine` without a path) is `form.field('').error`.

## Async checks: `rules`

For what a schema shouldn't hold: an API call, the app's state. A schema also runs on your server, where "is this email taken" is a database query.

```ts
useForm({
  schema: z.object({ username: z.string().min(3).max(20) }),
  defaultValues: { username: '' },
  debounce: 400,
  rules: {
    username: async (value, { signal }) =>
      (await isTaken(value, { signal })) ? 'That username is taken' : undefined,
  },
})
```

Other shapes: `'members.*.email': (value, { values, path }) => …` (`*` is any list index), `code: z.string().regex(/^[A-Z]{3}$/)` (a schema for that value), `handle: { on: 'blur', run: checkHandle, debounce: 0 }`.

- Runs only when the schema has no error at that path (a malformed email never calls your API), and not for the default value (an edit form doesn't re-check what the user already has).
- `debounce` waits for the last change; a newer change aborts the old call via `signal`; only the newest answer is used.
- Errors count like the schema's: `showErrors` gates them, they set `valid`, and `handleSubmit` waits for them and stops on one.
- `pending` is true meanwhile, on the field (`field('username').pending`) and on the form. Show it: `<Input :loading="pending" />`.
- Returns a message, a list, `msg('errors.taken')`, or nothing/`true` when fine. A throw (network down) stops the submit and rethrows.
- Cheap sync checks belong in the schema (`refine`), where they give feedback at once.
- The rules engine is a lazy chunk, loaded only for forms with `rules`.

## Without a schema library: `validate`

```ts
// shared rules: plain functions, written once
const required = (v) => !v && 'Required'
const pattern = (re, message) => (v) => !v || re.test(v) || message
const email = pattern(/^\S+@\S+\.\S+$/, 'Not an email')
const min = (n) => (v) => v >= n || `At least ${n}`

const form = useForm({
  defaultValues: { email: '', age: 0, password: '', confirm: '', members: [{ name: '' }] },
  validate: {
    email: [required, email], // in order; the first failure wins
    age: min(18),
    confirm: (v, { values }) => v === values.password || 'Must match',
    'members.*.name': required,
    '': (values) => values.members.length > 0 || 'Add a member', // an error no field owns
  },
  onSubmit: (values) => api.save(values),
})
```

- Types come from `defaultValues`; name them with `useForm<Values>({...})` when they can't be inferred (unions, optional fields).
- By convention a rule skips empty values (`!v || …`) so only `required` cares about empty.
- A rule gets `values`, so conditions are plain code: `iban: (v, { values }) => values.type === 'bank' && !v && 'Required'`.
- A rule may return a promise (the whole validation becomes async); for API calls prefer `rules` (debounce, abort).
- A rule can be a schema for one value (`email: z.email()`), alone or in the array; its errors use your catalog.
- Output is the values as they are: nothing stripped, trimmed or converted.
- Everything else is the same form: `showErrors`, `rules`, steps, `watch`, i18n.

Same rules on a server or in a test:

```ts
import { validator } from 'vael-form/validator'
export const signup = validator<Values>({ email: [required, email], age: min(18) })
// useForm({ schema: signup, defaultValues }), or on a server: await signup['~standard'].validate(body)
```

## Transforms

Transforms live in the schema; `field.value` stays what the user typed and `onSubmit` gets the output:

```ts
z.object({
  name: z.string().trim().min(1), // "  Ada " → "Ada"
  email: z.string().trim().toLowerCase().pipe(z.email()),
  hours: z.string().min(1).transform(Number).pipe(z.number().positive()), // text in, number out
  birthday: z.string().transform((value) => new Date(value)),
})
```

To change what's shown while typing (format a phone number), write to the field from `field.watch`: its own writes don't re-run it.

## Limits from the schema

`attrs` carries `minlength`/`maxlength` for text, `min`/`max`/`step` (`multipleOf`) for numbers, and `aria-required` for a value that can't be empty (a required string with a minimum, a required number; not a boolean or a plain string, which may be `''`). Zod, Valibot and ArkType only, loaded in a small chunk the first time `attrs` is read. A `maxlength` stops typing at the limit, so that error rarely shows; show a counter instead.

## Unions and dynamic schemas

- A discriminated union has the fields of every branch (`attrs.number`, `attrs.iban`); the schema validates the active one. The other branch's values are kept (switching back restores them); `onSubmit` gets only the active branch.
- `schema` can be a ref, for forms built from data: `form.model.kind.watch(({ value }) => (schema.value = schemas[value]))`. A new schema validates the same values again.
- A tuple is one field (`Field<[Date, Date]>`), its positions are paths (`range.0`). A recursive schema works; paths are typed to twelve levels.
