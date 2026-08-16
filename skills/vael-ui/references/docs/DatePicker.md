# DatePicker

A text field paired with a calendar popover for picking a date or range.

```ts
import { DatePicker } from 'vael-ui' // or 'vael-ui/vapor'
```

## Props

Name | Type | Default | Description
--- | --- | --- | ---
`selectionMode` | `CalendarSelectionMode \| undefined` | "single" | 
`view` | `CalendarView \| undefined` | "date" | 
`placeholder` | `string \| undefined` | undefined | 
`disabled` | `boolean \| undefined` | false | 
`invalid` | `boolean \| undefined` | false | 
`size` | `"md" \| "sm" \| "lg" \| undefined` | "md" | 
`minDate` | `Date \| undefined` | undefined | 
`maxDate` | `Date \| undefined` | undefined | 
`disabledDates` | `CalendarDisabledDates \| undefined` | undefined | 
`locale` | `string \| undefined` | undefined | 
`firstDayOfWeek` | `number \| undefined` | undefined | 
`formatOptions` | `Intl.DateTimeFormatOptions \| undefined` | undefined | Formats the trigger display. Default: `{ dateStyle: 'medium' }`.
`name` | `string \| undefined` | undefined | Hidden `<input>` mirroring selection as `YYYY-MM-DD` for form post.
`side` | `Side \| undefined` | "bottom" | 
`align` | `Align \| undefined` | "start" | 
`sideOffset` | `number \| undefined` | 8 | 
`alignOffset` | `number \| undefined` | 0 | 
`closeOnEsc` | `boolean \| undefined` | true | 
`closeOnOutside` | `boolean \| undefined` | true | 
`beforeClose` | `((done: () => void) => void) \| undefined` | undefined | 
`forceMount` | `boolean \| undefined` | false | 
`teleportTo` | `string \| HTMLElement \| undefined` | "body" | 
`motionCss` | `boolean \| undefined` | true | `false` skips popover enter/leave AND Calendar's month-slide transition.
`ui` | `Partial<{ root: UiPartValue; input: UiPartValue; positioner: UiPartValue; panel: UiPartValue; header: UiPartValue; navButton: UiPartValue; label: UiPartValue; weekdays: UiPartValue; weekday: UiPartValue; grid: UiPartValue; cell: UiPartValue; }> \| undefined` | undefined | 
`modelValue` | `Date \| CalendarRange \| null \| undefined` | null | 
`open` | `boolean \| undefined` | false | 

## Slots

_None._

## Events

Name | Type | Description
--- | --- | ---
`open-change` | `[value: boolean, details: PopoverOpenChangeDetails]` | 
`update:open` | `[value: boolean]` | 
`update:modelValue` | `[value: Date \| CalendarRange \| null]` | 
`change` | `[value: Date \| CalendarRange \| null]` | 
`month-change` | `[value: Date]` | 

## Exposed

Name | Type | Description
--- | --- | ---
`el` | `HTMLElement \| null` | 
`inputEl` | `HTMLInputElement \| null` | 
`panelEl` | `HTMLElement \| null` | 
`positionerEl` | `HTMLElement \| null` | 
`placement` | `Placement` | 
`positionerStyle` | `Record<string, string>` | 
`isClosing` | `boolean` | 
`close` | `() => void` | 
`cancelClose` | `() => void` | 

