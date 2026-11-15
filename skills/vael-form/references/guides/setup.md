# Setup: install, Vapor, i18n, Nuxt, SSR, stores

## Install

```sh
pnpm add vael-form zod   # or valibot, arktype, yup, joi, effect
```

Vue `^3.5.0` (or a `3.6` prerelease). Nothing from vael-ui is needed; vael-form works with plain inputs and any component library. A form needs no plugin: `useForm` alone works. Install `formPlugin` only for translated messages or an app-wide `showErrors`.

```ts
// main.ts
import { createApp } from 'vue'
import { createI18n } from 'vue-i18n'
import { formPlugin } from 'vael-form'

const i18n = createI18n({ legacy: false, locale: 'en', messages: { en, fr } }) // your catalogs, with a formKit block
const app = createApp(App)
app.use(i18n)
app.use(formPlugin, { i18n: i18n.global, showErrors: 'edited', keyPrefix: 'formKit' })
app.mount('#app')
```

- `i18n.global` works in composition and legacy mode. Without vue-i18n, pass any `{ t, locale }`: `t(key, params?, options?)` returning the key when missing, `locale` a ref or getter.
- One component tree with its own config: `provideFormConfig(computed(() => ({ i18n })))` in that tree's root setup.
- One form: `useForm({ i18n, showErrors })`. A form's `showErrors` replaces the app's, it isn't merged.

## VDOM and Vapor

Same package, same import, same API. It is tested in VDOM, pure Vapor (`createVaporApp`) and Vapor components inside a VDOM app (`vaporInteropPlugin`). A Vapor app installs the plugins the same way:

```ts
const app = createVaporApp(App as never)
app.use(i18n)
app.use(formPlugin, { i18n: i18n.global })
app.mount('#app')
```

In a `<script setup vapor>` component, `useForm` detects the component itself (Vue 3.6's instance probe), so ids still come from `useId()` and children still get the form through `useFormContext`.

## Nuxt

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@nuxtjs/i18n', 'vael-form/nuxt'],
  vaelForm: { showErrors: 'edited' }, // optional: showErrors, keyPrefix, i18n (true), autoImports (true)
})
```

The module installs `formPlugin` with the app's `$i18n` (read lazily, so module order doesn't matter) and auto-imports `useForm`, `useFormContext`, `useField` and `msg`. Your `formKit` messages go in the `@nuxtjs/i18n` locale files like any other keys. `i18n: false` leaves the forms untranslated; `autoImports: false` makes you import from `'vael-form'`.

## Server rendering

- Inputs get their ids from `useId()`, so server and browser agree (VDOM render + hydration is tested with no mismatch).
- Nothing shows as invalid on the server: no error text, no `aria-invalid`. Errors appear only once earned in the browser.
- A form made outside a component (store, module) gets ids from a counter: give it `id: 'signup'` when you server-render.
- Vapor has no server renderer yet, so SSR is only tested for VDOM.
- `defaultValues` can be a ref from `useFetch` / `useAsyncData`: the form takes it when it arrives.

## Outside a component

A form is a plain object (`markRaw`) that keeps its reactivity anywhere: a Pinia store, a composable, a module, a prop, `provide`.

```ts
export const useSignup = defineStore('signup', () => {
  const form = useForm({
    schema,
    id: 'signup',
    onSubmit: async (values, { form }) => {
      try {
        await api.signUp(values)
      } catch {
        form.setErrors({ email: 'That email is already registered' })
      }
    },
  })
  return { form }
})
```

```vue
<script setup lang="ts">
const { form } = useSignup()
const { model, errors, attrs, handleSubmit } = form
const { email } = model
</script>
```

- The app's `formPlugin` config reaches a form made in a Pinia store (Pinia sets stores up inside the app). In a bare module there is no app: pass `i18n` to `useForm`.
- Vue never wraps the form, so `store.form.values` can't be changed by accident. Change values through `model`, `setValue`, `setValues`.
- Refs stay refs: `form.valid.value` in a script.
- A form made outside a component isn't provided to children: `useFormContext()` only finds forms made in a parent component's setup.

## Size

About 8.2 KB gzipped. `rules`, the catalog readers, the schema's limits and the plain validator are separate chunks loaded only when a form uses them.
