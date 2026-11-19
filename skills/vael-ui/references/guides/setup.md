# Setup

Install, the two builds, auto-import, Vapor, Nuxt, SSR, hosts and fonts. Read this when starting an app or changing how it builds.


```sh
npm install vael-ui vue
```

Vue `^3.5.0`. The Vapor build needs Vue `^3.6.0` and `createVaporApp`; Vapor is pre-1.0, so **pin the exact prerelease** `vael-ui@0.5.5` is compiled against (`npm install vue@3.6.0-rc.15 --save-exact`). A floating `rc` tag causes compiler/runtime mismatches, often inside `<Transition>`. In Vapor a component's ref holds only what it exposes, not its element: when you pass your own component to `setTriggerEl`, `triggerEl` or `scrollEl`, give it `defineExpose({ el })`, or pass a ref to an element. Every vael-ui component with a root element exposes `el`.

```vue
<!-- App.vue -->
<script setup vapor lang="ts">
import { Button, Toaster, toast } from 'vael-ui/vapor'
</script>
<template>
  <Toaster />
  <Button @click="toast('Saved')">Save</Button>
</template>
```

| Need | Do |
| --- | --- |
| Not importing every component | `unplugin-vue-components` + `VaelUiResolver` from `vael-ui/resolver`: [Auto import](./auto-import.md) |
| Whole app as Vapor | `vue({ features: { vapor: true } })` (`@vitejs/plugin-vue` 6.0.8+); `optionsAPI: false` is safe. vue-tsc types an SFC as Vapor only with the `vapor` attribute, so cast the root: `createVaporApp(App as unknown as VaporComponent)`. With the `vapor` export condition add `"customConditions": ["vapor"]` to tsconfig, and spread `defaultClientConditions` / `defaultServerConditions` in Vite's `resolve.conditions` (alone, `['vapor']` breaks Vite) |
| Toasts, `v-tooltip`, `openDialog()`, `openPopover()` | Mount `<Toaster />`, `<TooltipHost />`, `<DialogHost />`, `<PopoverHost />` once, **inside** your `ConfigProvider` so what they show takes its theme, motion, messages and size |
| Fonts | Components use `font: inherit`: set `font-family` on `body` |
| Plain Vite SSR | `ssr: { noExternal: ['vael-ui'] }`, inject `ctx.teleports?.body` before `</body>`. `v-animate` entrances are written as CSS on the server and never replay on hydration |
| Nuxt | `vael-ui/nuxt` in `modules` (registers components and `v-animate`); `vaelUi: { variant: 'vapor' }` for the Vapor build (Nuxt 4.6+, Vue 3.6 pinned). Mount the hosts in `app.vue`, import composables from `'vael-ui'`, register `v-tooltip` in a plugin: `nuxtApp.vueApp.directive('tooltip', vTooltip)` |
| Fields zooming on iOS | Call `preventFocusZoom()` once in `main.ts`: iOS stops zooming a focused field, pinch-zoom still works, and fields keep your text size. Or raise field text on touch screens with `--ui-touch-font-size: 16px` |
| `vael-ui/style.css` | Don't import it by default; each component loads its own CSS |

