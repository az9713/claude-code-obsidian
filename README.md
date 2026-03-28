# Claude Code + Obsidian Workflows

> Claude Code skills and vault configuration for using Claude Code productively inside an Obsidian vault.

This repo contains six custom `vault-*` workflow skills built on top of the [Obsidian CLI](https://obsidian.md/cli), plus the official skills from [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills). Drop them into any Obsidian vault and Claude can read, write, search, and navigate your notes directly.

---

## Skills

### Custom vault-* skills (this repo)

Six skills designed for daily knowledge-base workflows:

| Skill | Slash command | What it does |
|-------|--------------|-------------|
| `vault-journal` | `/vault-journal` | Log a research entry to today's daily note — auto-searches for related vault projects and wikilinks them |
| `vault-capture` | `/vault-capture` | Give it a URL — extracts content via defuddle, creates a structured note with frontmatter and related links, files it into a new project folder |
| `vault-synthesize` | `/vault-synthesize` | Deep-search the vault on a topic, read the best sources, produce a cited synthesis note |
| `vault-moc` | `/vault-moc` | Generate a categorized Map of Content that indexes and wikilinks vault folders by theme |
| `vault-health` | `/vault-health` | Audit unresolved links, orphans, missing frontmatter — produce a prioritized cleanup report |
| `vault-save` | `/vault-save` | Capture a key insight from the current Claude Code conversation into a permanent vault note |

### From kepano/obsidian-skills

Five skills from [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) are included in `.claude/skills/`:

| Skill | What it does |
|-------|-------------|
| `obsidian-markdown` | Author Obsidian Flavored Markdown — wikilinks, embeds, callouts, properties, math, Mermaid |
| `obsidian-bases` | Create and edit `.base` files — database-like views with filters, formulas, and summaries |
| `json-canvas` | Create and edit `.canvas` files — nodes, edges, groups, flowcharts |
| `obsidian-cli` | Full reference for the Obsidian CLI — all commands, syntax, file targeting, dev tools |
| `defuddle` | Extract clean markdown from any web page using the Defuddle CLI |

---

## What's in this repo

```
README.md
CLAUDE.md                          # Vault conventions for Claude
claude-code-obsidian-workflows.md  # Full onboarding guide with example prompts
.claude/
  skills/
    vault-journal/SKILL.md
    vault-capture/SKILL.md
    vault-synthesize/SKILL.md
    vault-moc/SKILL.md
    vault-health/SKILL.md
    vault-save/SKILL.md
    obsidian-markdown/SKILL.md     # from kepano/obsidian-skills
    obsidian-bases/SKILL.md        # from kepano/obsidian-skills
    json-canvas/SKILL.md           # from kepano/obsidian-skills
    obsidian-cli/SKILL.md          # from kepano/obsidian-skills
    defuddle/SKILL.md              # from kepano/obsidian-skills
```

---

## Requirements

- [Obsidian](https://obsidian.md) desktop app — **must be running** for CLI commands that read vault data
- [Claude Code](https://claude.ai/code)
- Obsidian CLI enabled in **Settings → General → Command line interface**
- `npm install -g defuddle` — required for the `vault-capture` skill

> **Note:** Writing `.md` files directly to disk (used for long notes in `vault-capture`, `vault-synthesize`, `vault-moc`) works without Obsidian open. Obsidian's file watcher picks up the changes on next launch. All other CLI commands (`search`, `daily:append`, `tags`, etc.) require the app to be running.

---

## Installation

Copy `.claude/skills/` into the root of your Obsidian vault:

```bash
git clone https://github.com/az9713/claude-code-obsidian
cp -r claude-code-obsidian/.claude/skills/* /path/to/your/vault/.claude/skills/
```

Then copy `CLAUDE.md` to your vault root to give Claude vault-wide context:

```bash
cp claude-code-obsidian/CLAUDE.md /path/to/your/vault/CLAUDE.md
```

Open Claude Code from inside the vault folder — skills in `.claude/skills/` load automatically as project context.

---

## Context

These skills were developed for a knowledge base composed mostly of Claude Code and AI-related project folders — tutorials, experiments, agent workflows, and tool explorations. The `vault-*` skills are designed to scale to large vaults: hundreds of folders, tens of thousands of notes.

Read `claude-code-obsidian-workflows.md` for the full guide — how each skill was designed, example prompts, and what to expect as output.

---

## Credits

- Obsidian CLI and `kepano/obsidian-skills` by [Steph Ango](https://stephango.com/)
- `vault-*` workflow skills built with [Claude Code](https://claude.ai/code)
