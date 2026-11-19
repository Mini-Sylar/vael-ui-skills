# Every other export

The component and composable references cover each component's props and each composable's parameters. This page lists what's left in the package entry points (`vael-ui`, `vael-ui/vapor`, `vael-ui/motion`), so nothing exported is missing from the skill. Use it to find a helper or a type by name. Everything here exists in both builds.

Three kinds of export are not for app code, and are marked **internal** below: they're exported so `vael-ui/vapor` and the library's own components share one copy. Don't build on them: they may change in any release.

## Helpers you may use

| Export | What it does |
| --- | --- |
| `generateThemeCss(theme, scope = ':root')` | Turns a `UiTheme` (seed colours) into a `--ui-*` override block as a CSS string, for server-rendered or static theming without a `ConfigProvider`. Each colour sets one token and derives `-hover` and `-contrast` from it. |
| `askGuard(guard, context)` | Asks a `before-close` guard: resolves `true` to close, `false` to stay open. For a component of your own with the same guard. |
| `closeContext(reason, extra?)` | Builds the context a guard receives for a `reason`; `extra` carries a gesture's `progress` and `velocity`. |
| `useDOMTarget(target)` | Turns an element, a ref, a `useTemplateRef`, a getter or a selector into one element ref. The only place selectors are understood. |
| `resolveDOMTarget(target)` | The same, once, to an `HTMLElement` or `null`. |
| `resolvePastDisplayContents(el)` | The first descendant that generates a box, for a floating reference or a description when the element is `display: contents`. |
| `nativeTag(as)` | A native tag in capitals for `<component :is>`, so Vue doesn't resolve `'button'` to a registered `Button`. |
| `useUiMessages()` | The whole message catalogue from the nearest `ConfigProvider`, with English where it has none. |
| `defaultMessages`, `mergeMessages(base, overrides?)`, `resolveMessagesFromI18n(i18n)` | The English catalogue, merging a partial one over it, and building a partial one from your i18n's `t()` (a missing key falls back to English). For translating without a `ConfigProvider`. |
| `useClassMerge()`, `resolveUiPart(cx, part, ...classes)`, `splitUiPart(part)` | The `ui` prop's machinery for a component of your own: join built-in classes with a part's override (`class` and `style`), through the app's `class-merge`. `splitUiPart` splits without joining. |
| `useThemedUi(select, own)` | Reads the theme's `ui` for a component and your own `ui` prop together. |
| `markFocusVisible`, `showFocusVisible`, `clearFocusVisible`, `focusIsFromKeyboard()` | Focus handlers that draw a ring for keyboard focus only; `focusIsFromKeyboard()` inside a focus handler says whether keyboard travel produced it. |
| `ssrWindow`, `ssrDocument` | `window` and `document`, or `undefined` on the server, to pass as `useEventListener` targets. Never pass bare `window`. |
| `quarterCirclePoint(index, total, radius)` | Where SpeedDial puts action `index` of `total` on a quarter circle, as `{ x, y }`. |
| `tooltipTargets`, `TOOLTIP_ATTR` | The map and the attribute `v-tooltip` uses to find its targets. For a host of your own. |

## Sortable's pure functions

The decisions behind `useSortable` and `useSortableGroup`, exported so they can be tested or reused with plain numbers.

| Export | What it does |
| --- | --- |
| `excludeSubtree(rows, value)` | Drops a row and its subtree from a flat list. |
| `resolveInsertionIndex(bands, pointer)` | The linear slot for a pointer position along the sort axis. Bands in visual order. |
| `resolveDropPosition(input)` | A hovered slot and horizontal offset to a concrete `{ parentValue, index, depth }`. |
| `resolveRowShifts(remaining, ...)` | The px each row shifts once the dragged block is lifted out and put back at an index. |
| `resolveHoveredGroup(containers, point)` | Which registered container a point is over (inside a rect wins, else the nearest). |
| `resolveAdjacentGroup(order, currentId, direction)` | The next or previous container id in visual order, clamped; `null` if the id isn't there. |
| `moveTreeNode`, `isDescendantOf` | See [moveTreeNode](../composables/moveTreeNode.md). |

## Directive variants

`vTooltip`, `vScrollMask`, `vHoverIndicator`, `vDraggable` have a Vapor twin: `vTooltipVapor`, `vScrollMaskVapor`, `vHoverIndicatorVapor`, `vDraggableVapor`. Import the plain names from `vael-ui/vapor` in a Vapor app; they resolve to the Vapor twins. `vAnimate` and `vAnimateVapor` are exported from `vael-ui/motion`: in a `<script setup vapor>` component use `vAnimateVapor as vAnimate`.

## Injection keys

For code that reads or provides what a `ConfigProvider` or a component provides: `themeKey`, `themeScopeKey`, `messagesKey`, `classMergerKey`, `fieldKey` (the nearest `Field`'s context), `dialogRefKey`, `popoverRefKey` (the handle inside `openDialog` and `openPopover` content).

## Internal

Exported for the Vapor build and the library's own components. Not public API:

`useCloseGuard`, `useLayer`, `useInert`, `omitAttrs`, `useForwardedListener`, `normalizeText`, and the config helpers `defaultsKey`, `validLocale`, `withProviderDefaults`.

## Types

Grouped by the component or composable they belong to. Part types (`DialogPart` and the like) are the `ui` keys of their component, and the `*Motion` types are the shapes its `motion` prop takes.

### From `vael-ui`

- **Button:** `ButtonLoaderPlacement`, `ButtonSize`, `ButtonVariant`
- **ButtonGroup:** `ButtonGroupValue`
- **SplitButton:** `SplitButtonAlign`, `SplitButtonSide`
- **loaderType:** `LoaderSource`, `LoaderType`
- **Progress:** `ProgressSegment`, `ProgressVariant`
- **Message:** `MessageOpenChangeDetails`, `MessageProps`, `MessageVariant`
- **fieldContext:** `FieldContext`
- **PasswordInput:** `PasswordRule`, `PasswordRuleResult`
- **RadioGroup:** `RadioGroupContext`
- **SelectButton:** `SelectButtonItem`
- **Select:** `SelectAlign`, `SelectFilter`, `SelectGroupData`, `SelectGroupSlotData`, `SelectItemData`, `SelectSide`, `SelectVirtualizeConfig`
- **Combobox:** `ComboboxAlign`, `ComboboxCreateDetails`, `ComboboxCreateReason`, `ComboboxFilter`, `ComboboxSide`, `ComboboxTabBehavior`
- **FileUpload:** `FileRejectReason`
- **useDataTableContext:** `DataTableContext`, `RegisteredColumn`
- **Dialog:** `DialogExposed`, `DialogPosition`, `DialogProps`, `DialogSize`
- **CommandPalette:** `CommandPaletteItem`
- **BottomSheet:** `BottomSheetProps`, `SheetCloseReason`, `SheetSnapPoint`
- **Drawer:** `DrawerProps`, `DrawerSide`
- **Stepper:** `StepperItem`
- **Tour:** `TourEndDetails`, `TourGroup`, `TourProps`, `TourStep`, `TourStepChangeDetails`
- **Breadcrumb:** `BreadcrumbItemData`
- **Popover:** `PopoverAlign`, `PopoverExposed`, `PopoverProps`, `PopoverSide`
- **panelWidth:** `PanelWidth`
- **Menu:** `MenuAlign`, `MenuEntry`, `MenuItemData`, `MenuProps`, `MenuSeparator`, `MenuSide`
- **MenuList:** `MenuListItemData`, `MenuListProps`
- **ContextMenu:** `ContextMenuAlign`, `ContextMenuProps`, `ContextMenuSide`
- **Tooltip:** `TooltipAlign`, `TooltipProps`, `TooltipSide`
- **CascadeSelect:** `CascadeSelectFilter`, `CascadeSelectItem`, `CascadeSelectPath`
- **Tree:** `TreeNode`, `TreeSelectionMode`
- **TreeSelect:** `TreeSelectAlign`, `TreeSelectNode`, `TreeSelectSelectionMode`, `TreeSelectSide`
- **TooltipHost:** `TooltipHostProps`
- **Toaster:** `ToasterPosition`
- **theme:** `UiTheme`
- **useDialog:** `DialogCloseReason`, `DialogOpenChangeDetails`, `UseDialogOptions`
- **useDialogService:** `DialogRef`, `DynamicDialogEntry`, `OpenDialogHandle`, `OpenDialogOptions`
- **usePopoverService:** `DynamicPopoverEntry`, `OpenPopoverHandle`, `OpenPopoverOptions`, `PopoverRef`
- **confirmAction:** `ConfirmActionHandle`, `ConfirmActionOptions`
- **useNumberFormat:** `UseNumberFormatOptions`, `UseNumberFormatReturn`
- **useFieldControl:** `UseFieldControlOptions`, `UseFieldControlReturn`
- **useTabs:** `UseTabsOptions`, `UseTabsReturn`
- **useToolbar:** `UseToolbarOptions`, `UseToolbarReturn`
- **useCollapse:** `CollapseState`, `UseCollapseOptions`, `UseCollapseReturn`
- **useSheetDrag:** `UseSheetDragOptions`, `UseSheetDragReturn`
- **useTabIndicator:** `IndicatorMode`, `IndicatorSpring`, `UseTabIndicatorOptions`, `UseTabIndicatorReturn`
- **useHoverIndicator:** `UseHoverIndicatorOptions`
- **usePopover:** `PopoverCloseReason`, `PopoverOpenChangeDetails`, `UsePopoverOptions`
- **useMenu:** `UseMenuOptions`, `UseMenuReturn`
- **useKnob:** `UseKnobOptions`, `UseKnobReturn`
- **useDial:** `UseDialOptions`, `UseDialReturn`
- **useVirtualizer:** `ScrollAlign`, `UseVirtualizerOptions`, `UseVirtualizerReturn`, `VirtualRow`
- **useListbox:** `UseListboxOptions`, `UseListboxReturn`
- **useSlider:** `SliderOrientation`, `UseSliderOptions`, `UseSliderReturn`
- **useResizable:** `ResizeDirection`, `ResizeEdge`, `UseResizableOptions`, `UseResizableReturn`
- **useFileDrop:** `UseFileDropOptions`, `UseFileDropReturn`
- **useTooltip:** `TooltipCloseReason`, `TooltipOpenChangeDetails`, `UseTooltipOptions`
- **useFloatingPosition:** `Align`, `UseFloatingPositionOptions`
- **useAsyncLoading:** `UseAsyncLoadingReturn`
- **useHaptic:** `HapticImpact`, `HapticNotification`, `HapticOptions`, `UseHapticReturn`
- **types:** `AccordionMotion`, `AccordionPart`, `AnimatedNumberMotion`, `AnimatedNumberPart`, `AnimatedTextMotion`, `AnimatedTextPart`, `AvatarGroupMotion`, `AvatarGroupPart`, `AvatarMotion`, `AvatarPart`, `BadgeMotion`, `BadgePart`, `BeforeClose`, `BreadcrumbMotion`, `BreadcrumbPart`, `ButtonGroupMotion`, `ButtonGroupPart`, `ButtonMotion`, `ButtonPart`, `CalendarMotion`, `CardMotion`, `CardPart`, `CascadeSelectMotion`, `CheckboxMotion`, `CheckboxPart`, `ChipMotion`, `ChipPart`, `ChipsMotion`, `CloseContext`, `CollapsibleMotion`, `CollapsiblePart`, `ColorPickerMotion`, `ColorPickerPart`, `ComboboxMotion`, `CommandPaletteItemsMotion`, `CommandPaletteMotion`, `DataTableMotion`, `DatePickerMotion`, `DialMotion`, `DialPart`, `DialogMotion`, `DialogPart`, `DockMotion`, `DockPart`, `EditableMotion`, `EditablePart`, `FieldMotion`, `FieldPart`, `FileUploadMotion`, `GuardContext`, `InputMotion`, `InputNumberMotion`, `InputNumberPart`, `KbdMotion`, `KbdPart`, `KnobMotion`, `KnobPart`, `LoaderMotion`, `LoaderPart`, `MenuListMotion`, `MenuListPart`, `MenuMotion`, `MessageMotion`, `OtpInputMotion`, `PaginationMotion`, `PaginationPart`, `PasswordInputMotion`, `PasswordInputPart`, `PopoverMotion`, `PopoverPart`, `ProgressMotion`, `ProgressPart`, `PullToRefreshMotion`, `PullToRefreshPart`, `RadioGroupMotion`, `RadioGroupPart`, `RadioMotion`, `RadioPart`, `RatingMotion`, `RatingPart`, `ResizableMotion`, `ResizablePart`, `ScrollAreaMotion`, `ScrollAreaPart`, `SelectButtonMotion`, `SelectButtonPart`, `SelectMotion`, `SelectPart`, `SeparatorMotion`, `SeparatorPart`, `SkeletonMotion`, `SkeletonPart`, `SliderMotion`, `SliderPart`, `SortableMotion`, `SortablePart`, `SpeedDialMotion`, `StepperMotion`, `StepperPart`, `SwipeToRevealMotion`, `SwipeToRevealPart`, `SwitchMotion`, `SwitchPart`, `TabsMotion`, `TabsPart`, `TagMotion`, `TagPart`, `TextareaMotion`, `TextareaPart`, `TimelineMotion`, `ToasterMotion`, `ToolbarMotion`, `ToolbarPart`, `TooltipMotion`, `TourMotion`, `TreeMotion`, `TreePart`, `TreeSelectMotion`
- **config:** `UiDefaults`
- **useColorScheme:** `ColorSchemeMode`, `ColorSchemeRevealOptions`, `UseColorSchemeOptions`, `UseColorSchemeReturn`
- **useToast:** `ToastCall`, `ToastEntry`, `ToastFn`, `ToastId`, `ToastOptions`, `ToastVariant`
- **dom:** `DOMTarget`
- **useLayerStack:** `Layer`, `UseLayerOptions`
- **useScrollLock:** `UseScrollLockOptions`
- **useInert:** `UseInertOptions`, `UseInertReturn`
- **useTour:** `UseTourOptions`, `UseTourReturn`
- **messages:** `I18nInstance`, `PartialUiMessages`, `UiMessages`
- **classes:** `ClassMerger`, `ClassValue`, `UiPartStyle`, `UiPartValue`
- **vDraggable:** `DraggableOptions`, `DraggableValue`
- **vHoverIndicator:** `HoverIndicatorOptions`, `HoverIndicatorValue`
- **vTooltip:** `TooltipDirectiveOptions`, `TooltipDirectiveValue`
- **PullToRefresh:** `PullToRefreshProps`
- **usePullToRefresh:** `PullToRefreshState`, `UsePullToRefreshOptions`, `UsePullToRefreshReturn`
- **Dock:** `DockItemData`
- **useDock:** `DockFalloffOptions`, `DockOrientation`, `UseDockOptions`, `UseDockReturn`
- **Calendar:** `CalendarDisabledDates`, `CalendarRange`, `CalendarSelectionMode`, `CalendarView`
- **DatePicker:** `DatePickerAlign`, `DatePickerSide`
- **SpeedDial:** `SpeedDialDirection`, `SpeedDialItem`, `SpeedDialProps`, `SpeedDialTriggerMode`
- **useWheel:** `UseWheelOptions`, `UseWheelReturn`
- **usePanZoom:** `UsePanZoomOptions`, `UsePanZoomReturn`
- **useDragSelect:** `UseDragSelectOptions`, `UseDragSelectReturn`
- **useDrag:** `DragPoint`, `DragRelease`, `UseDragOptions`, `UseDragReturn`
- **useSwipeReveal:** `SwipeCommitInput`, `SwipeRevealSide`, `UseSwipeRevealOptions`, `UseSwipeRevealReturn`
- **sortableTree:** `TreeAccessors`
- **useSortable:** `DropPosition`, `FlatSortableRow`, `ResolveDropInput`, `SortableAnnounceEvent`, `SortableAxis`, `SortableBand`, `SortableDropDetails`, `SortableGroupHandle`, `SortableSource`, `SortableTreeNode`, `UseSortableOptions`, `UseSortableReturn`
- **useSortableGroup:** `GroupDropDetails`, `GroupDropPosition`, `UseSortableGroupOptions`

### From `vael-ui/motion`

- **animate:** `AnimateTarget`, `AnimationControls`, `SequenceAt`, `SequenceStep`, `SequenceStepOptions`
- **useScrollProgress:** `ScrollProgressOptions`
- **springEasing:** `MotionOptions`
- **useSpringValue:** `SpringHandle`, `SpringOptions`, `SpringState`, `UseSpringValueReturn`
- **types:** `AnimateDirectiveValue`, `AnimateModifier`, `AnimateOptions`, `AnimateStates`, `AnimateValues`, `BuiltinPreset`, `ComponentMotion`, `ElementHookContext`, `EngineTiming`, `LinkedMotion`, `ListContext`, `ListHookContext`, `LiveValue`, `MorphFrom`, `MotionContext`, `MotionEngine`, `MotionFilter`, `MotionHook`, `MotionMoment`, `MotionPhase`, `MotionPresets`, `MotionRef`, `MotionResult`, `MotionRole`, `MotionSpec`, `MotionStep`, `MotionStyle`, `MotionTiming`, `MotionValues`, `MotionWhere`, `MoveContext`, `OwnMotion`, `PartElements`, `PartValues`, `PresetName`, `Spring`, `SpringName`, `Stagger`, `StaggerFrom`, `StateMotion`, `TargetMotion`, `Thenable`, `TweenOptions`, `UiDuration`, `UiEase`, `UseMotionReturn`, `Viewport`
