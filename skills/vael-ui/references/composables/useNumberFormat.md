# useNumberFormat

Formats and parses numbers for a locale, with both directions kept in sync. `format` turns a number into the localized display string, including any affixes. `parse` turns typed text back into a number, or `null` if the text is incomplete or invalid. `isPartial` tells valid in-progress input (`''`, `'-'`, `'1.'`) apart from invalid text, so you don't reject input mid-typing. `InputNumber` uses it for its currency, percent and decimal modes.

```ts
import { useNumberFormat } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`locale` | `MaybeRefOrGetter<string \| undefined>` | Defaults to the runtime locale.
`mode` | `MaybeRefOrGetter<'decimal' \| 'currency' \| 'percent' \| undefined>` | Default `'decimal'`.
`currency` | `MaybeRefOrGetter<string \| undefined>` | ISO code, such as `'USD'`. Defaults to `'USD'` when `mode` is `'currency'`.
`minFractionDigits` | `MaybeRefOrGetter<number \| undefined>` | Passed to `Intl.NumberFormat` as `minimumFractionDigits`.
`maxFractionDigits` | `MaybeRefOrGetter<number \| undefined>` | Passed to `Intl.NumberFormat` as `maximumFractionDigits`.
`useGrouping` | `MaybeRefOrGetter<boolean \| undefined>` | Shows thousands separators. Default `true`.
`prefix / suffix` | `MaybeRefOrGetter<string \| undefined>` | A literal affix that `Intl` doesn't handle, such as a unit label. Added on format and stripped on parse.

## Returns

Name | Type | Description
--- | --- | ---
`format` | `(value: number \| null) => string` | Returns `''` for `null` or `NaN`. Otherwise returns the full localized string, including affixes.
`parse` | `(text: string) => number \| null` | Returns `null` for anything that isn't a complete, unambiguous number, including valid in-progress input.
`isPartial` | `(text: string) => boolean` | `true` for valid in-progress input that `parse` returns `null` for. Don't reject this input.
