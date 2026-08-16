# ConfigProvider

Wraps your app to provide theming, i18n, and class-merging to every component below it.

```ts
import { ConfigProvider } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`i18n` | `I18nInstance \| undefined` |  | I18n instance (e.g. `useI18n()` return value). Missing keys fall back to English defaults.
`messages` | `PartialUiMessages \| undefined` |  | Static message overrides, applied on top of `i18n` resolution.
`classMerge` | `ClassMerger \| undefined` |  | Class merger function (e.g. `twMerge` from tailwind-merge), applied to every `ui.*` part class.
`theme` | `UiTheme \| undefined` |  | Auto-derives a color system from seed colors and optional radius. Injects a scoped `<style>` tag.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `{}` | 

## Events

_None._

## Exposed

_None._

