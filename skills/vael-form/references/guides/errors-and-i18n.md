# Errors and translated messages

## When errors show

The schema always runs. `showErrors` only decides when an error becomes visible. Every mode shows all errors after a submit and keeps them live from then on.

| Mode | The error shows |
| --- | --- |
| `'edited'` (default) | once the field has been changed and left. Passing over an empty field says nothing |
| `'blur'` | once the field is left, even unchanged |
| `'input'` | as soon as the value changes, or once the field is left |
| `'submit'` | only after the first submit |

```ts
useForm({ schema, showErrors: 'input' })
useForm({ schema, showErrors: { default: 'edited', username: 'input', members: 'submit' } })
app.use(formPlugin, { showErrors: 'blur' }) // every form; a form's own value replaces it, not merged
```

- Per path: a group covers everything under it and the nearest path wins (`'members.0.name': 'input'` beats `members`). A top-level field named `default` can't be set apart.
- A row added to a list after a submit (or after a `validate(group)` step check) stays quiet until it's left, or until the next submit.
- `field.untouch()` hides a field's errors until it's left again; `field.touch()` marks it left immediately; `reset()` forgets everything.
- Server errors from `setErrors` aren't gated: they show at once.

Pick `'input'` for a field that needs live feedback while typing (a username with an async check); keep `'edited'` elsewhere.

## Reading errors

| Want | Read |
| --- | --- |
| The message under one input | `errors.email`, `errors.address.street`, `item.field.name.error` |
| A list's or group's own error ("add at least one") | `errors.members` (a list is one leaf), or `field('members').error` |
| An error no field owns (`refine` without a path, `''` in `validate`) | `field('').error` |
| Every message for a field | `field('password').errors` |
| A live checklist before the field is touched | `field('password').allErrors` |
| A summary at the top | `errorList` (shown errors, `{ path, message }`, in order) |
| Everything, shown or not | `allErrors`; `valid` is false when anything is wrong |
| Which step has problems | `field('profile').valid` |

Error summary linking to each field:

```vue
<ul v-if="errorList.length">
  <li v-for="e in errorList" :key="e.path">
    <a :href="`#${form.field(e.path as never).id}`">{{ e.message }}</a>
  </li>
</ul>
```

## Errors that move the page

An error that appears pushes what's below it down; a button that moves between mouse-down and mouse-up loses the click. vael-form waits for the pointer release before marking a field left by a press, so the click lands first; leaving by keyboard shows the error at once. Also reserve the line:

```vue
<p :id="attrs.email.errorId" class="error">{{ errors.email }}</p>
```

```css
.error { min-height: 1.5em; }
```

## Autofill

A browser autofill plays back focus, input, change and blur in a few milliseconds (Chrome does it at your first click). A field still holding an autofilled value (`:autofill` / `:-webkit-autofill`) doesn't count as left, so it shows nothing until the user types in it or submits.

## i18n

The library ships no translations. Connect your i18n once and add the form's messages to your own locale files under `formKit`:

```ts
app.use(i18n)
app.use(formPlugin, { i18n: i18n.global })
```

Your schemas stay as they are (`z.string().min(8)`). Zod, Valibot and ArkType report what went wrong and the form looks up the matching key when it shows the error. A locale switch updates errors already on screen without validating again; messages merged later (`mergeLocaleMessage`) show too.

The catalog (copy, translate, drop what you don't need):

```json
{
  "formKit": {
    "required": "{field} is required",
    "invalidType": {
      "default": "{field} is not valid",
      "string": "{field} must be text",
      "number": "{field} must be a number",
      "int": "{field} must be a whole number",
      "boolean": "{field} must be true or false",
      "date": "{field} must be a valid date",
      "array": "{field} must be a list"
    },
    "tooSmall": {
      "default": "{field} is too small (minimum {minimum})",
      "string": "{field} must be at least {minimum} characters",
      "number": "{field} must be at least {minimum}",
      "array": "{field} needs at least {minimum} items",
      "date": "{field} must be on or after {minimum}"
    },
    "tooBig": {
      "default": "{field} is too large (maximum {maximum})",
      "string": "{field} must be at most {maximum} characters",
      "number": "{field} must be at most {maximum}",
      "array": "{field} can have at most {maximum} items",
      "date": "{field} must be on or before {maximum}"
    },
    "invalidFormat": {
      "default": "{field} is not in the right format",
      "email": "Enter a valid email address",
      "url": "Enter a valid URL",
      "uuid": "{field} must be a valid UUID",
      "regex": "{field} is not in the right format",
      "startsWith": "{field} must start with \"{prefix}\"",
      "endsWith": "{field} must end with \"{suffix}\"",
      "includes": "{field} must contain \"{includes}\""
    },
    "invalidValue": "Choose one of the available options for {field}",
    "notMultipleOf": "{field} must be a multiple of {divisor}",
    "invalidUnion": "{field} is not valid",
    "unrecognizedKeys": "Unrecognized fields: {keys}",
    "custom": "{field} is not valid"
  }
}
```

- The most specific key is tried first (`tooSmall.string`, then `tooSmall.default`); a key your catalog lacks shows the library's own text.
- `required` covers a missing value, an empty string or list under `min(1)`, an empty text input where a number or date is expected, and (since 0.0.3) a choice not made yet: an unchosen radio or cleared select (Zod `enum`/`literal`, Valibot `picklist`/`literal`/`enum`/`variant`, ArkType unions of values). A value outside the options is `invalidValue`.
- Parameters: `{field}`, `{minimum}`, `{maximum}`, `{expected}`, `{format}`, `{divisor}`, `{keys}`, `{count}` (picks the plural form where the i18n library has one).
- `{field}` is the label from `labels` (itself a key: `labels: { email: 'labels.email' }`) or the name made readable (`firstName` → "First name").
- Move the namespace: `formPlugin, { i18n, keyPrefix: 'myForms' }`.
- The catalog readers load in a small lazy chunk; until they arrive (first error read, or at once when an i18n instance exists) the library's own text shows.
- A schema used on its own, outside a form, keeps its library's default text. Yup, Joi, Effect… always show their own text; use `msg` in those schemas.

## A message of your own

Write it in the schema, shown as written: `.min(8, 'Pick a longer name')`. To translate it, use `msg`:

```ts
import { msg } from 'vael-form'
z.string().refine(isFree, msg('errors.taken', { name }, 'Already taken')) // key, params, fallback
z.string().min(5, msg('errors.min', { min: 5 })) // '{field} needs {min}+ characters' in the catalog
msg(['errors.specific', 'errors.generic']) // keys tried in order
```

Register your keys so `msg` completes:

```ts
declare module 'vael-form' {
  interface Register {
    messageKeys: 'errors.taken' | 'errors.min'
  }
}
```

Per tree: `provideFormConfig(computed(() => ({ i18n })))`. Per form: `useForm({ i18n })`.
