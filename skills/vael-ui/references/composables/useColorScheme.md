# useColorScheme

Drives `document.documentElement.dataset.theme` from a `system` / `light` / `dark` mode. `system` removes the attribute entirely and follows `prefers-color-scheme` live. This is the exact composable the docs site’s own header theme toggle uses; `persist` is structural (like ConfigProvider’s `i18n`) so you can wire in cookies, a store, or localStorage yourself instead of the composable assuming one.

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

