# ConfigProvider

Wraps your app to provide theming, i18n, and class-merging to every component below it.

```ts
import { ConfigProvider } from 'vael-ui' // or 'vael-ui/vapor'
```

## Notes

With `i18n`, built-in strings come from your catalog under `uiKit.*`; a missing key keeps the English default. Keys added in 0.4.7: `uiKit.pagination.pageSize` ("Rows per page"), `uiKit.dataTable.loading` ("Loading…"), `uiKit.slider.minimum` ("Minimum") and `uiKit.slider.maximum` ("Maximum"). Keys added in 0.4.8: `uiKit.calendar.previousMonth` ("Previous month"), `uiKit.calendar.nextMonth` ("Next month"), `uiKit.calendar.previousYear` ("Previous year"), `uiKit.calendar.nextYear` ("Next year"), `uiKit.calendar.previousYears` ("Previous years") and `uiKit.calendar.nextYears` ("Next years"). The docs' I18n keys guide lists every key.

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`i18n` | `I18nInstance \| undefined` |  | I18n instance (e.g. `useI18n()` return value). Missing keys fall back to English defaults, and its current `locale` formats dates and numbers.
`messages` | `PartialUiMessages \| undefined` |  | Static message overrides, applied on top of `i18n` resolution.
`locale` | `string \| undefined` |  | BCP-47 locale for dates, numbers and the week start inside, e.g. `'de-DE'`. Wins over `i18n`'s locale. Set it when you render on a server, or they follow the server's language.
`classMerge` | `ClassMerger \| undefined` |  | Class merger function (e.g. `twMerge` from tailwind-merge), applied to every `ui.*` part class.
`theme` | `UiTheme \| undefined` |  | Auto-derives colors and applies radius and component font size. Injects a scoped `<style>` tag.
`size` | `"sm" \| "md" \| "lg" \| undefined` |  | Default size for buttons, form controls and tables inside. A component's own `size` wins.
`motionCss` | `boolean \| undefined` |  | `false` makes every component inside change instantly, with no built-in transitions, e.g. for screenshot tests. Panels still open and close and toasts still leave; a component's own `motionCss` wins.
`loaderType` | `'ring' \| 'dots' \| 'bars' \| 'comet' \| 'dither' \| undefined` | `'ring'` | Animation for every loader inside: Loader, Button's loading state, list loading rows, loading toasts and PullToRefresh. A component's own `type` or `loaderType` wins.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | App content that receives these settings.

## Events

_None._

## Exposed

_None._

