# Milda syntax, statement by statement

Every example below is checked against the reader of the language version this skill targets. A
fence marked `milda foundations`, `milda component` or `milda part` is a fragment: it goes inside a
`foundations { }` block, a `component { }` body, or a component body beside other parts. A fence
marked plain `milda` is a whole file or a top-level statement.

Words are closed sets. When this page and [vocabulary.md](vocabulary.md) disagree, vocabulary.md
is generated from the reader and wins.

## Lexical

- `#` starts a comment. Comments do not survive a round trip; anything that should survive has a
  statement of its own (descriptions are trailing strings).
- A block is `{ ... }`. Statements end at the end of a line, or at `;` inside one-line blocks:
  `layout { stack row; gap space.sm }`.
- A token is declared with `/` in its name (`space/sm`) and referenced with `.` (`space.sm`).
  A part is named with a dotted path; its prefix is its parent (`Header.Title`).
- "A name, then what it is" is always written with a colon: `token accent : color`,
  `label : string`, `part Title: text`.

## The header

```milda
milda 0.10
prelude 0.0.2
meta { name "Acme" }
```

`milda` is the surface version (which statements may appear), `prelude` the vocabulary of
archetypes, behaviours, facets and states. Write the current pair.

## Foundations: tokens

A token is `token <path> : <type> = <value>`. Types are closed (see vocabulary.md). Values:

```milda foundations
token brand : color = oklch(0.62 0.19 256)
token violet/600 : color = hex(7c3aed)
token radius/round : radius = pill
token fast : duration = 120ms
token standard : easing = raw "cubic-bezier(0.2, 0, 0, 1)"
token font/sans : font-family = { family: "Inter"; fallback: sans-serif; source: google; license: "OFL-1.1" }
token body : typography = { family: font.sans; size: 15px; weight: 400; line-height: 1.5 }
token leading/normal : ratio = 1.5
```

- Colours are written with a space-aware literal: `hex()`, `oklch()`, `rgb()`, `srgb()`, `p3()`,
  `hsl()`. Lengths are `px` (density-independent) or `scaled(...)` for text-relative sizes.
- `raw "..."` carries a value the literal forms cannot spell. In a record field that takes a value
  or a token, a structure is the value and a bare path is the token (`family: font.sans`).

Layers separate literals from meaning, and aliases answer per context:

```milda foundations
layer Base {
  token violet/500 : color = hex(8b5cf6)
  token violet/600 : color = hex(7c3aed)
}
layer Semantic {
  token accent : color = alias by ColorScheme {
    default -> violet.600
    Dark -> violet.500
  }
}
```

Derive instead of naming a second literal, and quarantine what only one platform can express:

```milda foundations
token accent : color = hex(7c3aed)
token accent-hover : color = compute accent in oklch { darken 10% }
token space/sm : spacing = 8px
token space/lg : spacing = compute space.sm { scale 2 }
token tint : color = escape "color-mix(in srgb, red 40%, blue)" note "web only, until compute can mix in srgb"
```

## Context groups

```milda
context-group ColorScheme { Light Dark }
context-group Density { Compact Comfortable }
```

A token aliases by a group (above). A prop typed `context Density` lets an instance choose.

## Types and shared props

```milda
type Option = object {
  label : string
  value : string
  disabled? : boolean
}

global-prop size : enum(sm, md, lg) = md "The control size, shared by every input"
```

## Components

```milda
component Badge "A small status pill." {
  archetype Badge
  group Feedback

  props {
    tone : enum(neutral, success) = neutral
    label : string = "New"
  }

  part Badge: container {
    fill accent-subtle
    corner radius.pill
  }

  part label: text {
    content bind label
  }
}
```

- `archetype` picks the prelude blueprint (see archetypes.md); it decides the required parts and
  the behaviours. A component without one is a pure composition.
- `group` files the component into a library section declared with
  `group Feedback "Telling a person what is happening."` at top level.

### Props, events, constraints

```milda component
props {
  tone : enum(neutral, danger) = neutral
  items : array<string>
  leading : component(Icon)
  label : string required "What the field asks for."
  error : string required when invalid "Shown while invalid."
  invalid : boolean = false
  icon : string
  labelledBy : string
  one-of label, labelledBy
}

events {
  valueChange
}
```

### Behaviours beyond the archetype's

```milda component
uses selection.single as S
uses presentation(surface: popover) as P
wire S.change -> P.close
gate P active when disabled=false
```

### Per-target realization

```milda component
realize web {
  presentation dialog { place center }
  backdrop scrim
  dismiss outside escape
  note "Focus is trapped while open."
}
```

## Parts

`part <path>: <kind> { ... }`. The kind is neutral (container, text, input, control, icon, item,
content, output, fragment, foreign, table, row, cell, instance). The root part carries the
component's name.

### Content, slots, presence

```milda part
part label: text {
  slot of content default sample "Get started"
}
part caption: text {
  content {
    default bind label
    when disabled -> "Unavailable"
  }
}
part icon: icon {
  present when icon and not loading
}
```

### Values, bindings, events

```milda part
part Field: container {
  bind state disabled disabled
  bind state error invalid
}
part control: input {
  value string
  bind value value
  emits {
    valueChange on input payload bind value
  }
}
```

`emits { <event> on <interaction> }` connects a declared event to a neutral interaction
(`activate`, `change`, `input`, `focus`, `blur`, `select`, `open`, `close`, `dismiss`).

### Accessibility

```milda part
part title: text {
  content "Email"
}
part hint: text {
  content "We never share it."
}
part control: input {
  a11y {
    labelled-by title
    described-by hint
  }
}
part name: text {
  presentation accessible-name
}
```

### Repetition, tables, media, links

```milda part
part option: item {
  repeat items as option
}
part heading: cell {
  cell header col
}
part avatar: content {
  media img
  source bind src sample url "https://example.com/a.png"
  alt bind name
}
part Link: container {
  destination bind href
}
```

### Controls, groups, behaviours on a part

```milda part
part toggle: control {
  control switch
}
part options: container {
  group segmented
}
part list: container {
  behavior roving-tabindex {
    orientation vertical
    loop
  }
}
```

### Composition with instances

```milda part
part status: instance {
  instance Badge {
    label = "Beta"
    tone = bind tone
  }
}
```

`tone = bind tone` passes this component's prop through; `label = "Beta"` fixes it.

### Paint (facets)

```milda part
part Card: container {
  fill surface
  ink text
  border line hairline
  border.block-end accent hairline
  corner radius.lg
  inset space.md space.lg
  inset.inline-start space.sm
  elevation shadow.card
  opacity 90
  text body
  weight weight.bold
}
```

`inset` takes one value (all edges), two (block, inline), or a single axis or logical edge with
`inset.inline`, `inset.block-end`, ... The most specific statement wins on its edge. Physical words
(`left`, `top`) are read but never written; write the logical edge.

### Layout

```milda part
part Toolbar: container {
  layout { stack row; gap space.sm; align center; distribute between; size fill fit }
}
```

- `stack row | column | layers` places the children. `layers` stacks them on top of each other; a
  child then takes `position overlay`, `pinned <edge>` or `bleed <edge>`.
- `size <width> <height>`, each `fit`, `fill`, `weight(n)`, a token (fixed) or `ratio(w:h)`, and
  an intent may carry `min(token)` / `max(token)`.
- There is no margin and no z-index: spacing is the parent's `gap`, order is the parts' order.

### States and conditions

```milda part
part Button: container {
  fill accent
  when variant = ghost {
    fill surface
  }
  state hovered {
    fill accent-hover
  }
  state hovered when variant = ghost {
    fill accent-subtle
  }
  state focus-visible {
    ring accent
  }
}
part icon: icon {
  when ancestor Button hovered or state focused { ink accent }
}
```

States come from behaviours (see vocabulary.md for the list and which behaviour emits each).

### Motion

```milda part
part Button: container {
  motion {
    transition fill duration fast easing standard
  }
}
```

### Foreign content (last resort)

```milda part
part chart: foreign {
  foreign web-react "Sparkline" "<Sparkline data={props.points} />"
}
```

A reader that does not implement the target skips it. Prefer a slot.

## Usage rules

```milda
rule no-badge-in-button warning {
  never archetype Badge within archetype Button
  because "A badge inside a button is read as part of its label."
}

rule no-stacked-dialogs error {
  never archetype Dialog within archetype Dialog
  because "A dialog over a dialog traps focus twice."
}

rule values-from-tokens warning {
  tokens color, spacing, radius
  because "Colours, spacing and radii come from the foundations."
}
```

A rule names components by their identifier (`never Badge within Card`) or by archetype
(`archetype Dialog`), and can count: `max 1 Button where variant = Primary within Card`. A rule
about a component the system does not declare matches nothing and is reported. Every rule says
`because`: the reason ships to people and agents with the rule.

## Examples (demo scenes)

```milda
example "Open dialog" for Dialog {
  demo {
    open : boolean = false
  }
  part trigger: control {
    action activate toggle open
  }
}
```

## Docs, assets, icons

```milda
assets {
  folder brand
  svg "logo" in brand {
    mime "image/svg+xml"
    view-box "0 0 24 24"
    url "assets/logo.svg"
  }
}

icons {
  set lucide "Lucide" { provider lucide; license ISC }
  glyph lucide check { label "Done"; svg "<path d='M20 6 9 17l-5-5'/>" }
  custom "logo-mark" { asset logo; label "Acme" }
}

docs {
  site { name "Acme"; tagline "One system, every surface" }
  footer { copyright "Acme, 2026" }
  page "Getting started" in "Guides" {
    heading "Getting started" anchor top
    paragraph "Install the package, import the theme, and build from the components."
  }
}
```

A docs page can transclude the system itself: `component Button`, `component-props Button`,
`component-events Button`, `component-group Actions`, `example "Open dialog"`.
