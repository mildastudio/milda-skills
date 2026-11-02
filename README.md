# Milda skills for AI assistants

An [Agent Skill](https://agentskills.io) that teaches an AI assistant the
[Milda](https://milda.dev) design-system language: how to read, write and fix `.milda` files,
and how to think in the language's terms - tokens in layers, context groups, archetypes,
platform-neutral parts and states that come from behaviours - rather than writing CSS in
Milda's syntax.

The skill tells the assistant to check every file it writes with the real reader
(`milda-check`, from the [`@mildastudio/milda-lsp`](https://www.npmjs.com/package/@mildastudio/milda-lsp)
package) and to fix what it reports, so it corrects itself from the language's own diagnostics
instead of from guesses.

## Install

**Claude Code**, as a plugin:

```
/plugin marketplace add mildastudio/milda-skills
/plugin install milda@milda
```

Third-party marketplaces do not auto-update by default; run `claude plugin update milda@milda`
after a new language version, or turn auto-update on for the marketplace.

**Any agent that reads Agent Skills** (Claude Code, Codex, Cursor, and others):

```bash
npx skills add mildastudio/milda-skills
```

**Claude on the web and desktop**: download this repository's `skills/milda` folder as a ZIP and
upload it under Customize > Skills > Create skill > Upload a skill. Or add the whole repository as
a plugin marketplace under Customize > Plugins.

**By hand**: copy `skills/milda` into your agent's skills directory (for Claude Code,
`~/.claude/skills/milda` or `.claude/skills/milda` in a project).

## What is in it

| Path                                   | What                                                                      |
| -------------------------------------- | ------------------------------------------------------------------------- |
| `skills/milda/SKILL.md`                | The ten rules of the paradigm, the write-check-fix loop, a CSS mapping    |
| `skills/milda/reference/syntax.md`     | Every statement, each with an example the reader accepts                  |
| `skills/milda/reference/vocabulary.md` | Every word the reader accepts, generated from the language package        |
| `skills/milda/reference/archetypes.md` | Every archetype with its contract and anatomy, generated from the prelude |
| `skills/milda/examples/acme.milda`     | A complete design system that checks clean                                |

## It moves with the language

This repository is mirrored from the Milda monorepo, where the skill is held to the reader on
every change: each example in it is parsed, the generated references are compared against a
fresh render of the language package, and every version it states must be the reader's. A
language change that would make the skill wrong fails the build until the skill is updated. The
skill targets the language surface named at the top of `SKILL.md`.

Issues and pull requests are welcome here; they are imported back into the monorepo.

## License

MIT
