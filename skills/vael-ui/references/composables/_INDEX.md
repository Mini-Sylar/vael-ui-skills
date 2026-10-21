# Composable reference index

## Overlays & Confirmation

- [confirmAction](./confirmAction.md): One function for async-aware confirm flows, either centered (`surface: 'dialog'`, the default) or anchored to a trigger (`surface: 'popover'`, requires `triggerEl`); a discriminated union on `surface` picks which options TypeScript actually offers you.
- [useDialogService](./useDialogService.md): The imperative engine behind `<Dialog>` for dialogs opened from code instead of markup.
- [usePopoverService](./usePopoverService.md): The imperative engine behind `<Popover>` for anchored popovers opened from code instead of markup.
- [useToast](./useToast.md): Sonner-style imperative toasts: call `toast(title, options)` from anywhere, no component context needed.
- [useTour](./useTour.md): The headless state machine behind `<Tour>` — index/group bookkeeping, step navigation, and the `onBeforeEnter`-await sequencing, with zero DOM or rendering baked in.

## Positioning & Data

- [useFloatingPosition](./useFloatingPosition.md): The Floating UI-backed positioning engine behind `Popover`, `Menu`, and `Tooltip`.
- [useVirtualizer](./useVirtualizer.md): Windowed rendering for long lists: only the visible rows (plus overscan) exist in the DOM.

## Motion

- [useSpringValue](./useSpringValue.md): vael-ui's spring engine: the physics behind Sortable, Tree and DataTable reordering, and behind every sliding indicator set to spring mode. Also exports `createSpring` and `stepSpring`.
- [useTabIndicator](./useTabIndicator.md): The sliding selection indicator behind Tabs, SelectButton, Pagination and MenuList, and behind the active option in Select, Combobox and CommandPalette.
- [useHoverIndicator](./useHoverIndicator.md): The sliding hover highlight behind every vael-ui list: Select, Menu, Tree, DataTable, Tabs, Pagination, Calendar and the rest.

## Utilities

- [useAsyncLoading](./useAsyncLoading.md): Tracks every in-flight promise passed to `run()`.
- [useColorScheme](./useColorScheme.md): Drives `document.documentElement.dataset.theme` from a `system` / `light` / `dark` mode.
- [useNumberFormat](./useNumberFormat.md): Locale-aware number formatting and parsing, both directions kept perfectly in sync.
- [useFieldControl](./useFieldControl.md): Wires a custom form control into the nearest `<Field>`: id/label association, `aria-describedby`/`aria-invalid`/`aria-required`, and reporting focus/filled state so Field can move a floating label or flip `data-filled`.
- [useScrollLock](./useScrollLock.md): Locks scrolling on an element, `document.body` by default.
- [preventFocusZoom](./preventFocusZoom.md): Stops iOS zooming into form fields under 16px by adding `maximum-scale=1` on iOS only; call it once in `main.ts`.

## Drag & Drop

- [useSortable](./useSortable.md): The spring-driven drag-to-reorder engine behind `<Sortable>`, `<Tree>`'s nested reorder, and `<DataTable>`'s column reorder — pointer and keyboard drive the same grabbed state, and all ordering/nesting decisions live in pure, independently-tested functions.
- [useSortableGroup](./useSortableGroup.md): Cross-container drag — the primitive a Kanban-style board is built from, not a component.
- [moveTreeNode](./moveTreeNode.md): Moves for a reorderable tree. Also exports `isDescendantOf`.
- [useFileDrop](./useFileDrop.md): The drag-and-drop half of FileUpload.

## Headless engines

Each drives one component and renders nothing. Reach for one when a component's slots can't give you the markup you need.

- [useTabs](./useTabs.md) (`<Tabs>`): The keyboard engine behind `<Tabs>`: selection, roving focus and arrow key navigation from the APG tabs pattern.
- [useMenu](./useMenu.md) (`<Menu>`): The keyboard engine behind `<Menu>`, `<MenuList>` and the SpeedDial actions: roving focus, wrapping arrow keys, Home and End, and typeahead.
- [useListbox](./useListbox.md) (`<Select>`): The active option and keyboard engine behind `<Select>` and `<Combobox>`.
- [useToolbar](./useToolbar.md) (`<Toolbar>`): The engine behind `<Toolbar>`: one Tab stop for the whole toolbar, arrow keys between its controls (APG toolbar pattern), and Priority+ overflow.
- [useCollapse](./useCollapse.md) (`<Collapsible>`): The height animation behind `<Collapsible>` and `<AccordionItem>`.
- [useTooltip](./useTooltip.md) (`<Tooltip>`): The headless tooltip behind `<Tooltip>`: open and close delays, a warm group in which the next tooltip opens at once and glides from the last one, hover onto the tooltip, Escape, and touch long-press.
- [useTooltipCore](./useTooltipCore.md) (`<Tooltip>`): `useTooltip` without its trigger hover, focus and press listeners, so one tooltip can serve many triggers through event delegation.
- [usePopover](./usePopover.md) (`<Popover>`): The headless popover behind `<Popover>`, `<Menu>`, `<Select>`, `<Combobox>`, `<DatePicker>` and `<TreeSelect>`.
- [useDialog](./useDialog.md) (`<Dialog>`): The headless modal behaviour behind `<Dialog>`: a cancelable close, Escape through the shared layer stack, a Tab focus trap, scroll lock, initial focus and focus return, and optional back-button dismiss.
- [useDataTableContext](./useDataTableContext.md) (`<DataTable>`): Reaches the nearest `<DataTable>`'s sort and selection state from a component rendered inside it, such as a custom cell.
- [useSlider](./useSlider.md) (`<Slider>`): The headless engine behind `<Slider>`.
- [useKnob](./useKnob.md) (`<Knob>`): The headless engine behind `<Knob>`: a value on a 270° sweep, from −135° to +135° with the gap at the bottom.
- [useDial](./useDial.md) (`<Dial>`): The headless engine behind `<Dial>`: a value you turn like a jog wheel, with no fixed sweep.
- [useResizable](./useResizable.md) (`<Resizable>`): The headless drag and keyboard engine behind `<Resizable>`.
- [useDock](./useDock.md) (`<Dock>`): The magnification engine behind `<Dock>`.
- [dockFalloff, dockItemSize, dockItemSizes](./dockFalloff.md) (`<Dock>`): The pure, DOM-free size math `useDock` runs on every pointer move.
- [useSwipeReveal](./useSwipeReveal.md) (`<SwipeToReveal>`): The drag engine behind `<SwipeToReveal>`.
- [resolveSwipeCommit](./resolveSwipeCommit.md) (`<SwipeToReveal>`): The pure open or close decision `useSwipeReveal` makes on release.
- [useSheetDrag](./useSheetDrag.md) (`<BottomSheet>`): The drag and snap engine behind `<BottomSheet>`, modeled on Vaul.
- [usePullToRefresh](./usePullToRefresh.md) (`<PullToRefresh>`): The gesture engine and state machine behind `<PullToRefresh>`.
