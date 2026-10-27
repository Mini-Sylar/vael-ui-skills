# Calendar

A month grid for picking a single date, multiple dates, or a range.

```ts
import { Calendar } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`selectionMode` | `'single' \| 'range' \| undefined` | `'single'` | `'range'` picks a start then an end; clicking before the start restarts the range.
`view` | `'date' \| 'month' \| 'year' \| undefined` | `'date'` | What a click selects: a day, a month (its 1st) or a year (January 1st).
`minDate` | `Date \| undefined` |  | Earliest selectable date (inclusive); the calendar disables earlier days, months and years.
`maxDate` | `Date \| undefined` |  | Latest selectable date (inclusive); the calendar disables later days, months and years.
`disabledDates` | `CalendarDisabledDates \| undefined` |  | List of unavailable dates, or a predicate function. Matching compares calendar days and ignores time.
`locale` | `string \| undefined` |  | BCP-47 locale for month and weekday names and the week start day. Unset, it uses the runtime default.
`firstDayOfWeek` | `number \| undefined` |  | `0` (Sunday) to `6` (Saturday). Unset, it derives from `locale` and falls back to Monday.
`months` | `number \| undefined` | `1` | How many months to show side by side, for picking a range at a glance. They move together and wrap onto separate rows when there isn't room.
`showYearButtons` | `boolean \| undefined` | `false` | Adds « and » beside the month arrows to jump a year at a time, handy for ranges that span years. Only in the day view; the month and year grids already step by years.
`showWeekNumbers` | `boolean \| undefined` | `false` | Shows each week's number (ISO 8601) at the start of its row.
`motion` | `CalendarMotion \| undefined` | `true` | `false` skips the month change's slide. An object restyles it: give the `label` and `grid` looks as values, or as functions of `where.direction` (`1` forward, `-1` back).
`ui` | `Partial<{ root: UiPartValue; header: UiPartValue; navButton: UiPartValue; label: UiPartValue; weekdays: UiPartValue; weekday: UiPartValue; grid: UiPartValue; cell: UiPartValue; }> \| undefined` |  | Class and style overrides for each part.
`modelValue` | `Date \| CalendarRange \| null \| undefined` | `null` | Selected date, or `{ start, end }` in `range` mode.

## Slots

Name | Type | Description
--- | --- | ---
`label` | `{ label: string; date: Date; level: CalendarView; }` | Custom content for the month and year heading (defaults to its text). The heading keeps its own click-to-pick-month/year, keyboard and aria wiring. With `months`, each month's label.
`day` | `{ date: Date; isCurrentMonth: boolean; isToday: boolean; isSelected: boolean; isDisabled: boolean; }` | Custom content inside each day cell (defaults to the day number). The cell keeps its own click, keyboard and aria wiring.

## Events

Name | Type | Description
--- | --- | ---
`update:modelValue` | `[value: Date \| CalendarRange \| null]` | Fires when `modelValue` changes (`v-model`).
`change` | `[value: Date \| CalendarRange \| null]` | Fires when you pick a date, month, year or range endpoint.
`month-change` | `[value: Date]` | Fires when the displayed month changes (nav buttons, keyboard, or clicking an adjacent month's day).

## Exposed

Name | Type | Description
--- | --- | ---
`rootEl` | `HTMLElement \| null` | Root element.
`gridEl` | `HTMLElement \| null` | Day grid element (`null` while the month or year grid shows).
`highlightEl` | `HTMLElement \| null` | The hover and keyboard highlight, sliding between cells. With `motion: false` it moves at once, for your own animation.
`goToPreviousMonth` | `() => void` | Steps back one page: a month, or a year or 12 years in the month and year grids.
`goToNextMonth` | `() => void` | Steps forward one page: a month, or a year or 12 years in the month and year grids.
`focusDay` | `(day: Date) => void` | Moves keyboard focus to the given day, switching months if needed. With `view` set to `'month'` or `'year'`, focuses its month or year.

## Motion

`motion` takes `CalendarMotion` (exported from `'vael-ui'`); `false` turns it off. How Calendar's label and grid change months. Give each part's look as values, or as a function of `where`: its `direction` is `1` going forward and `-1` going back.

- Parts: `label`, `grid`
- Keys: `spring`, `duration`, `easing`, `initial`, `exit`, `onAfterSwap`

See the [Motion guide](../guides/motion.md) for what each key does.

