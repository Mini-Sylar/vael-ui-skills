# Calendar

A month grid for picking a single date, multiple dates, or a range.

```ts
import { Calendar } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`selectionMode` | `CalendarSelectionMode \| undefined` | "single" | 
`view` | `CalendarView \| undefined` | "date" | 
`minDate` | `Date \| undefined` | undefined | 
`maxDate` | `Date \| undefined` | undefined | 
`disabledDates` | `CalendarDisabledDates \| undefined` | undefined | List of unavailable dates, or a predicate function. Matched by calendar day, ignoring time.
`locale` | `string \| undefined` | undefined | BCP-47 locale for month/weekday names and week start day. Omitted uses runtime default.
`firstDayOfWeek` | `number \| undefined` | undefined | 0 (Sunday) – 6 (Saturday). Omitted derives from `locale`, falling back to Sunday.
`motionCss` | `boolean \| undefined` | true | `false` skips month-navigation slide transition.
`ui` | `Partial<{ root: UiPartValue; header: UiPartValue; navButton: UiPartValue; label: UiPartValue; weekdays: UiPartValue; weekday: UiPartValue; grid: UiPartValue; cell: UiPartValue; }> \| undefined` | undefined | 
`modelValue` | `Date \| CalendarRange \| null \| undefined` | null | 

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: Date \| CalendarRange \| null]` | 
`change` | `[value: Date \| CalendarRange \| null]` | 
`month-change` | `[value: Date]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`rootEl` | `HTMLElement \| null` | 
`gridEl` | `HTMLElement \| null` | 
`goToPreviousMonth` | `() => void` | 
`goToNextMonth` | `() => void` | 
`focusDay` | `(day: Date) => void` | 

