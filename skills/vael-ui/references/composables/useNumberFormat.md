# useNumberFormat

Formats and parses numbers for a locale, with both directions kept in sync. `format` turns a number into the localized display string, including any affixes. `parse` turns typed text back into a number, or `null` if the text is incomplete or invalid. `isPartial` tells valid in-progress input (`''`, `'-'`, `'1.'`) apart from invalid text, so you don't reject input mid-typing. `InputNumber` uses it for its currency, percent and decimal modes.

```ts
import { useNumberFormat } from 'vael-ui' // or 'vael-ui/vapor'
```

## Parameters

Name | Type | Description
--- | --- | ---
`locale` | `MaybeRefOrGetter<string \| undefined>` | The locale to format for. Default: the runtime locale.
`mode` | `MaybeRefOrGetter<'decimal' \| 'currency' \| 'percent' \| undefined>` | The format style. Default `'decimal'`.
`currency` | `MaybeRefOrGetter<string \| undefined>` | The ISO currency code. Default `'USD'` when `mode` is `'currency'`.
`minFractionDigits` | `MaybeRefOrGetter<number \| undefined>` | The fewest decimals to show, passed to `Intl.NumberFormat` as `minimumFractionDigits`.
`maxFractionDigits` | `MaybeRefOrGetter<number \| undefined>` | The most decimals to show, passed to `Intl.NumberFormat` as `maximumFractionDigits`.
`useGrouping` | `MaybeRefOrGetter<boolean \| undefined>` | Shows thousands separators. Default `true`.
`prefix / suffix` | `MaybeRefOrGetter<string \| undefined>` | Adds literal text that `Intl` doesn't handle, such as a unit label. `format` adds it and `parse` strips it.

## Returns

Name | Type | Description
--- | --- | ---
`formatOptions` | `ComputedRef<Intl.NumberFormatOptions>` | The `Intl.NumberFormat` options built from `mode`, `currency`, the fraction digits and grouping. Pass them to AnimatedNumber for the same format.
`format` | `(value: number \| null) => string` | Turns a number into the full localized string, affixes included. Returns `''` for `null` or `NaN`.
`parse` | `(text: string) => number \| null` | Turns typed text back into a number. Returns `null` for anything that isn't a complete, unambiguous number, including valid in-progress input.
`isPartial` | `(text: string) => boolean` | `true` for valid in-progress input that `parse` returns `null` for. Don't reject this input.

