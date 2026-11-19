# useColorScheme

Sets the root `data-theme` to `light` or `dark`; `system` follows the OS preference. Pass `persist` to save the choice in cookies, a store or `localStorage`. Vue restores a saved choice on mount, which can be after first paint, so the HTML head example restores the color mode, and an optional root font size, before paint. Every call in the browser shares one state, so two toggles never disagree. Pass `{ from: event }` as the second argument to `setMode` and the new scheme spreads in a circle from the click, as the example below and this site's header toggle do. It uses a View Transition; browsers without one, and reduced motion, switch at once.

```ts
import { useColorScheme } from 'vael-ui' // or 'vael-ui/vapor'
```

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

// mode.value: 'system' | 'light' | 'dark'
// resolvedMode.value: 'light' | 'dark'
// From your theme toggle, spreading from the click:
// <button @click="(e) => setMode(resolvedMode === 'dark' ? 'light' : 'dark', { from: e })">
```

## Parameters

Name | Type | Description
--- | --- | ---
`initial` | `'system' \| 'light' \| 'dark'` | The mode to use until `persist.get()` is read on mount. Default `'system'`.
`persist` | `{ get: () => string \| null; set: (mode) => void }` | Saves and restores the choice. Without it, nothing is saved. `set` receives `null` for `'system'`.

## Returns

Name | Type | Description
--- | --- | ---
`mode` | `ShallowRef<'system' \| 'light' \| 'dark'>` | The selected mode.
`resolvedMode` | `ShallowRef<'light' \| 'dark'>` | The applied theme, with `'system'` resolved against the live media query.
`setMode` | `(mode, reveal?: { from: Event \| Element \| { x, y }; duration?; easing? }) => void` | Sets the mode, persists it and applies it. With `reveal`, the new scheme spreads in a circle from `from`: a click, an element, or a point. `duration` defaults to 500 ms.

