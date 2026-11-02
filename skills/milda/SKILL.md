---
name: milda
description: Read, write, review and fix Milda design systems - `.milda` files - and reason in Milda's paradigm (tokens in layers, context groups, archetypes, platform-neutral parts, states from behaviours). Use whenever the user mentions Milda, Milda Studio, the Milda playground or a `.milda` file, asks to author or change a design system's tokens or components in Milda, or wants to translate CSS, Figma or an existing component library into Milda.
---

# Milda

Milda is a compiler with a GUI for design systems. A `.milda` file **is** the design system:
foundations (tokens), components, usage rules, docs and assets. React packages, Figma libraries,
docs sites and agent-readable files are generated from it. You never edit an output; you change
the file and regenerate.

This skill targets language surface **0.10** with prelude **0.0.2**. Every file you write starts
with that header.

## The loop: write, check, fix

Never tell the user a `.milda` file is valid until the reader agrees. After every edit, run:

```bash
npx -y -p @mildastudio/milda-lsp milda-check --strict path/to/system/
```

Give it the directory, or every file of the system together, because tokens declared in one file
resolve references in another. It prints `path:line:col severity [code] message`, the same
diagnostics the editor extensions show, and exits non-zero on any error (or, with `--strict`, any
warning). Fix what it reports and run it again until it says `0 errors, 0 warnings`. A warning is
not noise: `unknown facet "background" skipped` means that line did nothing.

To show the user the result, the playground at https://play.milda.dev renders a pasted file live,
with no account.

## Think in Milda: ten rules

Most mistakes come from writing CSS, a DOM tree or a React component in Milda's syntax. These rules
are what the syntax exists to express.

1. **The system is the source; code is an output.** Everything platform-shaped is decided at
   generation time, so a file never holds a CSS property, a tag, an event name of a framework or a
   pseudo-class.
2. **Say what a thing is, never what it is made of.** A part has a neutral _kind_ (`container`,
   `text`, `input`, `icon`, `control`, ...), never `div` or `button`. Each target picks its own
   element.
3. **A value has a name, and the name has a layer.** Components never paint with literals. A facet
   points at a token; a `Semantic` layer aliases a `Base` layer of literals. A restyle is a change
   to foundations.
4. **Context is a dimension, not a copy.** Dark mode, density, brand: each is a `context-group`, and
   a token answers per context with `alias by Group { ... }`. A prop typed by a context group (a
   `variant`) uses the same mechanism per instance.
5. **A state comes from a behaviour.** `hovered`, `pressed`, `focus-visible`, `disabled`, `open`,
   `selected` are emitted by the archetype's behaviours. Style them with `state hovered { }`, never
   with anything pointer- or platform-specific. The list is closed (`reference/vocabulary.md`), and
   the reader warns on any other word: `state hover` is reported with the spelling it meant.
6. **The contract is the identity.** `props` and `events` are what consumers code against and what
   versioning diffs. Name them for meaning (`variant`, `checkedChange`), not for a platform.
7. **Build from what exists.** Start a component from an `archetype` in the prelude: it brings the
   contract shape, required parts and behaviours (keyboard, ARIA, focus). Compose larger
   components from smaller ones with `instance` parts.
8. **Every reference is by name.** Parts by dotted path (`Header.Title`), tokens by path
   (`space.sm` for `space/sm`), components by identifier. No ids. Comments (`#`) are the only
   thing that does not survive a round trip.
9. **Extend through seams, never by patching output.** When the vocabulary cannot say something,
   use a `slot`, a per-target option, or as a last resort `foreign` content tagged with its target.
   Never suggest editing generated code.
10. **Say how it is meant to be used.** Put interface constraints (`required`, `required when`,
    `one-of`) in `props`, and composition rules (`rule ... { never X within Y }`) at top level,
    each with its reason.

## A file, top to bottom

```milda
milda 0.10
prelude 0.0.2

meta { name "Acme" }

context-group ColorScheme { Light Dark }

foundations {
  layer Base {
    token violet/600 : color = hex(7c3aed)
    token white : color = hex(ffffff)
    token gray/900 : color = hex(0f172a)
  }
  layer Semantic {
    token accent : color = alias by ColorScheme { default -> violet.600 }
    token on-accent : color = alias by ColorScheme { default -> white; Dark -> gray.900 }
    token space/sm : spacing = 8px
    token radius/sm : radius = 6px
  }
}

component Button "The primary action." {
  archetype Button

  props {
    label : string = "Get started"
  }

  events {
    click
  }

  part Button: container {
    emits { click on activate }
    layout { stack row; gap space.sm; align center }
    fill accent
    ink on-accent
    corner radius.sm
    inset space.sm
    state focus-visible {
      ring accent
    }
  }

  part label: text {
    content bind label
  }
}
```

Order: header, `meta`, `context-group`s, `group`s (library sections), `foundations`, `type`s,
`component`s, `rule`s, then `example`s, `assets`, `icons`, `docs`. The root part is named after the
component; other parts are named by the archetype's anatomy (`label`, `icon`, `control`, ...) or
by you, nested with dotted paths.

## From CSS and React to Milda

| You would write                             | Milda says                                              |
| ------------------------------------------- | ------------------------------------------------------- |
| `<div>`, `<span>`, `<button>`               | `part X: container` / `text` / the archetype's root     |
| `background`                                | `fill`                                                  |
| `color`                                     | `ink`                                                   |
| `padding`, `padding-inline-start`           | `inset`, `inset.inline-start`                           |
| `margin`                                    | nothing: `gap` on the parent, alignment, or `bleed`     |
| `border`, `border-radius`                   | `border <color> <width>`, `corner`                      |
| `box-shadow`, `outline` (focus)             | `elevation`, `ring`                                     |
| `font-size` + `line-height`, `font-weight`  | `text <typography or font-size token>`, `weight`        |
| `display: flex; flex-direction: row`        | `layout { stack row }`                                  |
| `justify-content`, `align-items`, `flex: 1` | `distribute`, `align`, `size fill`                      |
| `position: absolute`                        | `layout { stack layers }` + `position overlay` on child |
| `:hover`, `:focus-visible`, `:disabled`     | `state hovered { }`, `state focus-visible { }`, ...     |
| `.btn--secondary`, `[data-variant]`         | `when variant = Secondary { }`                          |
| `.card:hover .icon`                         | `when ancestor Card hovered { }` on the icon part       |
| `@media (prefers-color-scheme: dark)`       | `context-group ColorScheme` + `alias by ColorScheme`    |
| `--space-sm: 8px` / `var(--space-sm)`       | `token space/sm : spacing = 8px` / `space.sm`           |
| `#7c3aed`, `oklch(...)`                     | `hex(7c3aed)`, `oklch(...)`, only inside a token        |
| `color-mix()`, `calc()`                     | `compute <token> in oklch { darken 10% }`               |
| `onClick` prop                              | `events { click }` + `emits { click on activate }`      |
| `{children}`                                | `slot of content default` on the part that holds them   |
| `{cond && <X/>}`                            | `present when <prop>` on the part                       |
| `z-index`, `text-align`, `box-sizing`       | nothing: part order, the container, there is one box    |

## Facets, states and conditions on a part

```milda part
  part Panel: container {
    layout { stack column; gap space.sm; size fill fit }
    fill surface
    ink text
    border line hairline
    corner radius.sm
    inset space.sm space.md
    when tone = danger {
      border danger hairline
    }
    state hovered {
      fill surface-raised
    }
    state hovered when tone = danger {
      fill danger-subtle
    }
  }
```

A facet's value resolves as a token first; a bare literal is accepted only where no token claims
the word (`opacity 50`, `inset 0`). Prefer tokens. `when` keys on props, `state` on behaviour
states, and the two combine.

## Props, events and constraints

```milda component
  archetype TextField

  props {
    label : string required "What the field asks for. Also its accessible name."
    value : string
    invalid : boolean = false
    error : string required when invalid "Shown under the control while invalid."
    size : enum(sm, md) = md
    tone : context Tone = Neutral
  }

  events {
    valueChange
  }
```

Types: `string`, `number`, `boolean`, `enum(a, b)`, `context <Group>`, `array<T>`, your own `type`s.
A trailing string is the prop's documentation. `one-of a, b` / `any-of` / `exclusive` constrain
groups of props.

## Where to look next

- [reference/syntax.md](reference/syntax.md): every statement with a checked example (tokens and
  values, layout, slots, instances, behaviours, rules, docs, assets, icons).
- [reference/vocabulary.md](reference/vocabulary.md): generated from the language package. Every
  token type, part kind, facet, state, layout word and literal function the reader accepts.
  When in doubt about a word, look here instead of guessing.
- [reference/archetypes.md](reference/archetypes.md): generated from the prelude. Every archetype
  with its contract, anatomy (which parts are required) and behaviours.
- [examples/acme.milda](examples/acme.milda): a complete, clean system to copy patterns from.
- The normative spec: https://github.com/mildastudio/milda-lang/tree/main/spec (`FORMAT.md` for
  what a file promises, `GRAMMAR.ebnf` for the syntax, `PRELUDE.md` for the archetypes).
- For a design system that is already published, the Milda MCP server answers with its exact
  components and tokens; see https://milda.dev/mcp.

## Before you finish

- The header says `milda 0.10` and `prelude 0.0.2`.
- No literal colour, length or font in a component; every value is a token reference.
- No platform word anywhere: no tags, CSS properties, pseudo-classes or framework event names.
- Every component starts from an archetype unless it is a pure composition of instances.
- `milda-check --strict` on the whole system prints `0 errors, 0 warnings`.
