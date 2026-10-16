# ConfigProvider

Wraps your app to provide theming, i18n, and class-merging to every component below it.

```ts
import { ConfigProvider } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`i18n` | `I18nInstance \| undefined` |  | I18n instance (e.g. `useI18n()` return value). Missing keys fall back to English defaults, and its current `locale` formats dates and numbers.
`messages` | `PartialUiMessages \| undefined` |  | Static message overrides, applied on top of `i18n` resolution.
`classMerge` | `ClassMerger \| undefined` |  | Class merger function (e.g. `twMerge` from tailwind-merge), applied to every `ui.*` part class.
`theme` | `UiTheme \| undefined` |  | Auto-derives colors and applies radius and component font size. Injects a scoped `<style>` tag.
`size` | `"sm" \| "md" \| "lg" \| undefined` |  | Default size for buttons, form controls and tables inside. A component's own `size` wins.
`motionCss` | `boolean \| undefined` |  | `false` makes every component inside change instantly, with no built-in transitions, e.g. for screenshot tests. Panels still open and close and toasts still leave; a component's own `motionCss` wins.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | App content that receives these settings.

## Events

_None._

## Exposed

_None._

