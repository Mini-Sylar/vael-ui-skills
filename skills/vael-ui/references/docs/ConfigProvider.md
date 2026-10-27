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
`motion` | `boolean \| MotionOptions \| MotionStyle \| undefined` |  | Motion for everything inside. `false` makes every component change instantly, e.g. for screenshot tests: panels still open and close and toasts still leave. `{ spring }` puts drawers, sheets, toasts, sliding indicators and AnimatedNumber on one spring (below 1, damping overshoots); crossfades such as AnimatedText keep their own timing. `{ valueSwap: false }` shows a picked value in Select, Combobox, TreeSelect, CascadeSelect and DatePicker without its crossfade. A full style (`{ version: 1, … }`) sets how every role and component moves, defined once. A component's own `motion` wins.
`loaderType` | `LoaderSource \| undefined` | `'ring'` | Animation for every loader inside: Loader, Button's loading state, list loading rows, loading toasts and PullToRefresh. A built-in name, or your own component (or render function), drawn in the same 1em box. A component's own `type` or `loaderType` wins.
`indicator` | `IndicatorMode \| undefined` | `'glide'` | How every sliding indicator inside moves: the hover highlight in lists and menus, and the selection in Tabs, SelectButton, Pagination and MenuList. `'glide'`, `'instant'`, or a spring such as `{ damping: 0.55, response: 0.3 }` (below 1, damping overshoots). `motion: false` still makes them instant.
`haptics` | `boolean \| HapticOptions \| undefined` | `undefined (yours play, vael-ui's stay silent)` | Haptics on phones that offer them. `true`, or `{ intensity }` to scale them, turns on vael-ui's own: a Switch flipping, a swiped row snapping, a Sortable row landing, a pull passing the refresh point, a held Button completing. `false` silences every `useHaptic` call inside too, yours included.

## Slots

Name | Type | Description
--- | --- | ---
`default` | `any` | App content that receives these settings.

## Events

_None._

## Exposed

_None._

