# useColorScheme

Drives `document.documentElement.dataset.theme` from a `system` / `light` / `dark` mode. `system` removes the attribute entirely and follows `prefers-color-scheme` live. This is the exact composable the docs site’s own header theme toggle uses; `persist` is structural (like ConfigProvider’s `i18n`) so you can wire in cookies, a store, or localStorage yourself instead of the composable assuming one. Being a plain composable, it can only apply the saved mode once your JS bundle runs a component's `setup()` — after first paint. A returning user with a saved `dark` preference will see a flash of the light theme first unless you also set the attribute synchronously before Vue mounts; see the pre-hydration snippet below.

## Example

```ts
import { useColorScheme } from 'vael-ui'

const { mode, resolvedMode, setMode } = useColorScheme({
  initial: 'system',
  persist: {
    get: () => localStorage.getItem('theme'),
    set: (mode) => {
      if (mode) localStorage.setItem('theme', mode)
      else localStorage.removeItem('theme')
    },
  },
})

// mode.value: 'system' | 'light' | 'dark' (what the user picked)
// resolvedMode.value: 'light' | 'dark' (what's actually applied right now)
setMode('dark')

// Avoiding a flash of the wrong theme on load
// ---------------------------------------------
// useColorScheme can't run before Vue mounts, so a returning user with a
// saved preference sees the default theme for a frame first. Set the
// attribute synchronously in a blocking <script> in your HTML's <head>,
// before your app's bundle loads — same trick as next-themes/Nuxt color-mode.
// Match whatever persist.get()/set() convention you wired up above.
//
    <script>
       (function () {
         var saved = localStorage.getItem('theme') // your own persist.get()
         if (saved === 'light' || saved === 'dark') {
           document.documentElement.dataset.theme = saved
         }
       })()
     </script>
```

## Parameters

Name | Type | Description
--- | --- | ---
`initial` | `'system' \| 'light' \| 'dark'` | Starting mode before persist.get() (if any) resolves. Default 'system'.
`persist` | `{ get: () => string \| null; set: (mode) => void }` | Structural persistence hook. No default, nothing is persisted unless you pass this.

## Returns

Name | Type | Description
--- | --- | ---
`mode` | `ShallowRef<'system' \| 'light' \| 'dark'>` | What the user picked.
`resolvedMode` | `ShallowRef<'light' \| 'dark'>` | What's actually applied. Resolves 'system' against the live media query.
`setMode` | `(mode) => void` | Sets mode, persists it, and re-applies.

