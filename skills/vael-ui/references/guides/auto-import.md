# Auto import

`vael-ui/resolver` is a resolver for `unplugin-vue-components`. Register it once and every vael-ui component becomes usable in a template with no manual import at all.

## Setup

```ts
// vite.config.ts
import Components from 'unplugin-vue-components/vite'
import { VaelUiResolver } from 'vael-ui/resolver'

export default defineConfig({
  plugins: [
    Components({
      resolvers: [VaelUiResolver()],
    }),
  ],
})
```

That's it: write the tag, the import gets inserted at build time. `unplugin-vue-components` supports Vite, Webpack, Rollup, esbuild, and Rspack; the resolver itself is a plain function, works identically regardless of which one is used.

**No extra CSS wiring needed.** Every vael-ui component already imports its own CSS internally, so resolving the component import pulls its styles in for free. (Most `unplugin-vue-components` resolvers need a separate `sideEffects` entry to also import a component's stylesheet; vael-ui's doesn't.)

## Vapor

vael-ui ships two builds from the same source: the regular VDOM build and a Vapor build (`vael-ui/vapor`). By default the resolver imports from the VDOM build; pass `variant: 'vapor'` to resolve from the Vapor build instead:

```ts
VaelUiResolver({ variant: 'vapor' })
```

Pick whichever the app actually renders with; mixing the two in one app isn't supported (aside from Vue's own vdom/vapor interop for gradual migration, which is a separate, more advanced setup).

Known gap: the resolver doesn't currently scan `<script setup vapor>` blocks for auto-import candidates the way it scans plain `<script setup>`. If a component silently isn't auto-importing inside a vapor-marked SFC, that's this limitation, not a misconfiguration.

## One shared import, no `/vapor` subpath

If an editor's auto-import keeps suggesting `vael-ui` instead of `vael-ui/vapor` (a known TypeScript limitation when a package exports the same name from two entry points), set a `vapor` export condition instead of importing from the subpath. Every `import … from 'vael-ui'` then resolves to the Vapor build project-wide, components and composables alike, and there's nothing left for auto-import to pick wrong.

```ts
// vite.config.ts
export default defineConfig({
  resolve: { conditions: ['vapor'] },
})
```

```jsonc
// tsconfig.json
{
  "compilerOptions": {
    "moduleResolution": "bundler",
    "customConditions": ["vapor"]
  }
}
```

**Keep both settings in sync.** The Vite condition controls what actually loads; the TS condition just keeps types matching it. Set one without the other and the result is a silent mismatch (correct types with the wrong build loaded, or vice versa), not an error.
