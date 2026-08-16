# useNumberFormat

Locale-aware number formatting and parsing, both directions kept perfectly in sync. `format` turns a number into the full localized+affixed display string, `parse` turns typed text back into a number (or `null` for anything incomplete or invalid), and `isPartial` tells a legal mid-typing state ('', '-', '1.') apart from actual garbage so you don’t fight the user while they’re still typing. This is exactly what `InputNumber` uses internally for its own currency/percent/decimal modes.

## Parameters

Name | Type | Description
--- | --- | ---
`locale` | `MaybeRefOrGetter<string \| undefined>` | Defaults to the runtime locale.
`mode` | `MaybeRefOrGetter<'decimal' \| 'currency' \| 'percent' \| undefined>` | Default 'decimal'.
`currency` | `MaybeRefOrGetter<string \| undefined>` | ISO code, e.g. 'USD'. Default 'USD' when mode is currency.
`minFractionDigits` | `MaybeRefOrGetter<number \| undefined>` | Passed to Intl.NumberFormat.
`maxFractionDigits` | `MaybeRefOrGetter<number \| undefined>` | Passed to Intl.NumberFormat.
`useGrouping` | `MaybeRefOrGetter<boolean \| undefined>` | Thousands separators. Default true.
`prefix / suffix` | `MaybeRefOrGetter<string \| undefined>` | Literal affix Intl has no concept of (e.g. a unit label); stripped on parse, appended on format.

## Returns

Name | Type | Description
--- | --- | ---
`format` | `(value: number \| null) => string` | null/NaN -> ''. Always the full localized+affixed string.
`parse` | `(text: string) => number \| null` | null for anything that isn’t a complete, unambiguous number, including legal in-progress typing states.
`isPartial` | `(text: string) => boolean` | True for legal mid-typing states parse correctly returns null for but that must not be rejected.

