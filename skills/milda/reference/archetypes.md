<!-- Generated from @mildastudio/milda by `npm run gen:skills`. Do not edit: edit the prelude, then regenerate. -->

# Milda archetypes

Prelude **0.0.2**: 51 archetypes. Start a component with
`archetype <Name>`. A part not marked optional is required: the reader restores it if a file
leaves it out, and `milda-check` reports it. A trailing `?` on a behaviour means the archetype
composes it only when the component asks for it.

## Actions

### Button

label/icon, fires action.

- Parts: `icon` (icon, optional), `label` (text)
- Behaviours: `activation`, `pressability`, `async-state?`

### ToggleButton

on/off action.

- Parts: `indicator` (icon, optional), `label` (text)
- Behaviours: `toggle`, `pressability`

### MenuButton

opens a menu of actions.

- Parts: `trigger` (control) > `label` (text), `indicator` (icon, optional), `surface` (container) > `item` (item) > `label` (text)
- Behaviours: `presentation`, `browse`, `typeahead?`, `dismiss`

### Link

navigates.

- Parts: `label` (text), `icon` (icon, optional)
- Behaviours: `activation`, `pressability`

## Inputs

### Form

a set of fields, one submit.

- Parts: `content` (container), `submit` (control) > `label` (text), `error` (text, optional)
- Behaviours: `submission`

### Field

label + control + error/help wrapper.

- Parts: `label` (text), `control` (container), `help` (text, optional), `error` (text, optional)
- Behaviours: `validation`

### TextField

string value.

- Parts: `icon` (icon, optional), `control` (input)
- Behaviours: `value-entry`, `validation?`

### TextArea

multiline string value.

- Parts: `control` (input)
- Behaviours: `value-entry`, `validation?`

### SearchField

query string + clear.

- Parts: `icon` (icon, optional), `control` (input), `clear` (control, optional)
- Behaviours: `value-entry`, `filter?`

### BooleanInput

one bool.

- Parts: `control` (input), `indicator` (icon), `label` (text, optional)
- Behaviours: `toggle`

### ChoiceGroup

one-of-N.

- Parts: `option` (item) > `indicator` (icon), `label` (text)
- Behaviours: `selection.single`, `browse`

### SingleSelect

one of a known set.

- Parts: `trigger` (control) > `value` (text), `indicator` (icon), `surface` (container) > `option` (item) > `check` (icon, optional), `label` (text)
- Behaviours: `selection.single`, `presentation`, `browse`, `dismiss`, `typeahead?`
- Emits: `change`

### MultiSelect

subset of a set.

- Parts: `trigger` (control) > `value` (text), `indicator` (icon), `surface` (container) > `option` (item) > `check` (icon), `label` (text)
- Behaviours: `selection.multiple`, `presentation`, `browse`, `dismiss`

### Combobox

filtered single-select w/ entry.

- Parts: `control` (input), `surface` (container) > `option` (item) > `label` (text)
- Behaviours: `value-entry`, `filter`, `selection.single`, `browse`, `presentation`, `dismiss`

### Slider

value(s) on a range.

- Parts: `track` (container) > `range` (container, optional), `thumb` (control), `value` (text, optional)
- Behaviours: `range`, `gesture(pan)`

### Stepper

number ±.

- Parts: `decrement` (control) > `icon` (icon), `control` (input), `increment` (control) > `icon` (icon)
- Behaviours: `stepping`, `value-entry`

### DatePicker

date value.

- Parts: `trigger` (control) > `value` (text), `icon` (icon), `surface` (container) > `header` (container) > `prev` (control), `title` (text), `next` (control), `grid` (container) > `day` (item) > `label` (text)
- Behaviours: `selection.single`, `presentation`, `browse`, `dismiss`, `masking`

### TimePicker

time value.

- Parts: `trigger` (control) > `value` (text), `icon` (icon), `surface` (container) > `column` (item) > `option` (item) > `label` (text)
- Behaviours: `selection.single`, `presentation`, `browse`, `dismiss`, `masking`

### ColorPicker

color value.

- Parts: `trigger` (control) > `swatch` (icon), `value` (text), `surface` (container) > `area` (container), `hue` (container), `alpha` (container), `eyedropper` (control), `input` (input), `swatches` (container) > `swatch` (item)
- Behaviours: `range`, `value-entry`, `presentation`, `dismiss`

### FileUpload

file(s).

- Parts: `dropzone` (container) > `icon` (icon), `label` (text), `input` (input), `list` (container, optional) > `item` (item) > `name` (text), `remove` (control)
- Behaviours: `activation`, `gesture(drop)?`, `async-state`

### TagsInput

list of tokens.

- Parts: `tag` (item) > `label` (text), `remove` (control), `control` (input)
- Behaviours: `value-entry`, `selection.multiple`

### Rating

discrete score.

- Parts: `item` (item) > `icon` (icon)
- Behaviours: `selection.single`, `pressability`

## Disclosure

### Disclosure

expand region (a.k.a. Collapsible).

- Parts: `trigger` (control) > `label` (text), `indicator` (icon), `panel` (container)
- Behaviours: `disclosure`

### Accordion

group of disclosures (single/multi).

- Parts: `item` (item) > `header` (control) > `label` (text), `indicator` (icon), `panel` (container)
- Behaviours: `disclosure`, `selection`

### Tabs

switch panels.

- Parts: `list` (container) > `tab` (control) > `label` (text), `panel` (container)
- Behaviours: `navigation`, `browse`

### Dialog

blocking layer (a.k.a. Modal).

- Parts: `trigger` (control) > `label` (text), `backdrop` (container, optional), `surface` (container) > `title` (text), `content` (container), `close` (control, optional)
- Behaviours: `presentation(dialog)`, `focus-scope`, `dismiss`, `backdrop`

### Drawer

edge-anchored panel (a.k.a. Sidebar).

- Parts: `trigger` (control), `backdrop` (container, optional), `surface` (container) > `content` (container), `close` (control, optional)
- Behaviours: `presentation(sheet)`, `focus-scope`, `dismiss`, `gesture(swipe)?`

### Popover

anchored transient layer.

- Parts: `trigger` (control), `surface` (container) > `content` (container)
- Behaviours: `presentation(popover)`, `anchoring`, `dismiss`

### Tooltip

hover/focus hint.

- Parts: `trigger` (control), `surface` (container) > `content` (text)
- Behaviours: `presentation(popover)`, `anchoring`

### ContextMenu

secondary-action menu.

- Parts: `surface` (container) > `item` (item) > `icon` (icon, optional), `label` (text)
- Behaviours: `presentation(menu)`, `browse`, `dismiss`

## Navigation

### Menu

hierarchical actions (a.k.a. MenuBar).

- Parts: `surface` (container) > `item` (item) > `icon` (icon, optional), `label` (text)
- Behaviours: `presentation`, `browse`, `typeahead?`, `dismiss`

### NavigationBar

top-level destinations.

- Parts: `item` (item) > `icon` (icon, optional), `label` (text)
- Behaviours: `navigation`

### Breadcrumbs

path trail.

- Parts: `item` (item) > `label` (text), `separator` (icon, optional)
- Behaviours: `navigation`

### Pagination

page index.

- Parts: `previous` (control), `item` (item) > `label` (text), `next` (control)
- Behaviours: `navigation`, `stepping`

### Tree

hierarchical collection.

- Parts: `item` (item) > `indicator` (icon), `label` (text)
- Behaviours: `disclosure`, `browse`, `selection`

### Wizard

ordered flow (a.k.a. Stepper-nav).

- Parts: `step` (item) > `indicator` (icon), `label` (text), `content` (container), `back` (control), `next` (control)
- Behaviours: `stepping-nav`

## Collections

### List

ordered items, optional select (a.k.a. ListBox).

- Parts: `item` (item) > `content` (container)
- Behaviours: `browse`, `selection?`, `virtualization?`

### Table

rows×cols, sort/select (a.k.a. DataGrid).

- Parts: `header` (container) > `column` (item) > `label` (text), `body` (container) > `row` (item) > `cell` (item)
- Behaviours: `selection?`, `ordering`, `browse`, `resize?`, `virtualization?`

### Carousel

paged horizontal items.

- Parts: `viewport` (container) > `slide` (item), `previous` (control, optional), `next` (control, optional)
- Behaviours: `browse`, `momentum-scroll`, `gesture(swipe)`

### Grid

2D arrangement of items.

- Parts: `item` (item)
- Behaviours: `browse`, `selection?`

## Feedback

### Toast

transient message (a.k.a. Snackbar).

- Parts: `icon` (icon, optional), `content` (text), `action` (control, optional), `close` (control, optional)
- Behaviours: `presentation`, `auto-dismiss`, `liveness`

### Alert

inline persistent message (a.k.a. Banner).

- Parts: `icon` (icon, optional), `title` (text, optional), `content` (text), `close` (control, optional)
- Behaviours: `liveness`

### ProgressBar

completion feedback (a.k.a. Spinner).

- Parts: `track` (container) > `indicator` (container), `label` (text, optional)
- Behaviours: `progress`, `liveness`

### Skeleton

loading placeholder.

- Parts: the root only
- Behaviours: `async-state`

### Badge

numeric/status decoration.

- Parts: `label` (text)

### Meter

numeric gauge.

- Parts: `track` (container) > `indicator` (container), `label` (text, optional)

### EmptyState

no-content placeholder.

- Parts: `icon` (icon, optional), `title` (text), `description` (text, optional), `action` (control, optional)

## Media

### Avatar

identity image/initials.

- Parts: `image` (content), `fallback` (text, optional), `badge` (icon, optional)

### Media

image/video/audio/embed w/ alt + fit (proposal 0028).

- Parts: `media` (content)

### Icon

vector glyph from a set.

- Parts: the root only

### AspectRatio

ratio-locked box.

- Parts: `content` (container)
