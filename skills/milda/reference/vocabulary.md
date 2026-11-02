<!-- Generated from @mildastudio/milda by `npm run gen:skills`. Do not edit: edit the language package, then regenerate. -->

# Milda vocabulary

Surface **0.10**, prelude **0.0.2**. These are the closed word sets
the reader accepts; a word that is not here is not Milda.

## Token types

`color`, `spacing`, `radius`, `size`, `dimension`, `shadow`, `typography`, `font-family`, `font-weight`, `ratio`, `line-height`, `easing`, `gradient`, `font-size`, `letter-spacing`, `duration`, `opacity`, `blur`, `tonal`, `border`, `elevation`, `ring`, `backdrop`, `none`

A token's value is a literal, or one of `raw`, `alias`, `escape`, `compute`.

## Value literals

- Colour: `hex(...)`, `oklch(...)`, `srgb(...)`, `rgb(...)`, `p3(...)`, `hsl(...)`, and the keyword `transparent`.
- Length: `px`, and `scaled(...)`.

## Part kinds

`container`, `item`, `text`, `control`, `input`, `output`, `icon`, `content`, `fragment`, `foreign`, `table`, `row`, `cell`, `instance`

## Statements inside a part

`kind`, `origin`, `present`, `slot`, `content`, `a11y`, `emits`, `bind`, `value`, `repeat`, `cell`, `control`, `group`, `presentation`, `media`, `source`, `alt`, `destination`, `layout`, `state`, `when`, `motion`, `instance`, `behavior`, `foreign`, `action`, `severity`, `accordion`, `calendar`, `time-picker`, `tabs`, `slider`, `rating`, `color-picker`, `overlay`

## Facets

| Facet | Covers |
| ----- | ------ |
| `fill` | surface/background |
| `ink` | text + icon foreground |
| `border` | edge: color + width (hairline or thin or thick) + style |
| `border.top` | top edge (physical side) |
| `border.right` | right edge (physical side) |
| `border.bottom` | bottom edge (physical side) |
| `border.left` | left edge (physical side) |
| `border.inline-start` | leading edge (logical, writing-direction) |
| `border.inline-end` | trailing edge (logical, writing-direction) |
| `border.block-start` | block-start edge (logical) |
| `border.block-end` | block-end edge (logical) |
| `corner` | corner radius (token role) |
| `inset` | internal padding (block/inline) |
| `inset.block` | `inset`, narrowed to block |
| `inset.inline` | `inset`, narrowed to inline |
| `inset.block-start` | `inset`, narrowed to block-start |
| `inset.block-end` | `inset`, narrowed to block-end |
| `inset.inline-start` | `inset`, narrowed to inline-start |
| `inset.inline-end` | `inset`, narrowed to inline-end |
| `gap` | spacing between children |
| `text` | typography role (body or caption or title…) - bundles size/weight/line/tracking |
| `text.clamp` | `text`, narrowed to clamp |
| `text.wrap` | `text`, narrowed to wrap |
| `elevation` | depth |
| `overlay` | state layer (Material hover/press tint) |
| `opacity` | transparency |
| `ring` | focus indicator |
| `backdrop` | scrim / blur behind layer |
| `weight` | font-weight (typography axis §3, split out of the `text` role bundle) |
| `font` | font-family (typography axis §3) |
| `slant` | font-style: normal  or  italic  or  oblique (typography axis §3) |

Inset edges: `block`, `inline`, `block-start`, `block-end`, `inline-start`, `inline-end`. Logical edges: `block-start`, `block-end`, `inline-start`, `inline-end`, `block`, `inline`, `all`.

## Layout

| Key | Values |
| --- | ------ |
| `stack` | `row`, `column`, `layers` |
| `flow` | a token or value |
| `gap` | a token or value |
| `align` | `start`, `center`, `end`, `stretch`, `baseline` |
| `distribute` | `start`, `center`, `end`, `between`, `around`, `evenly` |
| `wrap` | `nowrap`, `wrap`, `wrap-reverse` |
| `position` | `flow`, `overlay`, `pinned`, `bleed` |
| `size` | `width`, `height`, `fit`, `fill`, `weight`, `fixed`, `ratio` |
| `ratio` | a token or value |
| `inset` | a token or value |
| `grid` | `cols`, `rows` |

Axis intents: `fit`, `fill`, `weight`, `fixed`, `ratio`, each optionally clamped with `min`, `max`.
Positions: `flow`, `overlay`, `pinned`, `bleed`.

## States

| State | Emitted by |
| ----- | ---------- |
| `hovered` | pressability |
| `pressed` | pressability |
| `focused` | value-entry |
| `focus-visible` | value-entry (keyboard focus) |
| `active` | browse |
| `selected` | selection |
| `checked` | toggle |
| `indeterminate` | toggle (tristate) |
| `expanded` | disclosure |
| `open` | presentation |
| `disabled` | ambient (global gate) |
| `readonly` | ambient |
| `loading` | async-state |
| `error` | async-state |
| `success` | async-state |
| `first` | collection-position |
| `last` | collection-position |
| `odd` | collection-position |
| `even` | collection-position |
| `today` | the calendar core (DatePicker day) |
| `outside-month` | the calendar core (DatePicker day) |
| `range-start` | the calendar core (range mode) |
| `range-end` | the calendar core (range mode) |
| `in-range` | the calendar core (range mode) |
| `highlighted` | browse (the calendar cursor) |

## Interactions (`emits { <event> on <interaction> }`)

`activate`, `change`, `input`, `focus`, `blur`, `select`, `open`, `close`, `dismiss`

## Smaller closed sets

- Slot arities: `single`, `optional`, `many`
- Control realizations (`control`): `checkbox`, `switch`
- Group realizations (`group`): `radio`, `segmented`
- Presentation (`presentation`): `accessible-name`
- Media types (`media`): `img`, `video`, `audio`, `embed`
