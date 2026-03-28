# CLAUDE.md — _claude_code Vault

This is an Obsidian vault containing ~312 project folders, mostly Claude Code and AI-related projects — tutorials, experiments, tool explorations, and research notes. It serves as a personal knowledge base for the Claude Code ecosystem.

## Vault Structure

- **Root level:** Project folders + daily notes (`YYYY-MM-DD.md`) — no other loose files
- **Each project folder** typically contains some combination of:
  - `README.md` — project summary or main note
  - `transcript.txt` — raw source transcript (YouTube, conversation, etc.)
  - `gemini3_summary.txt` — AI-generated analysis/summary of the content
  - `CLAUDE.md` — Claude Code project instructions (for code projects)
  - Actual code, images, and other assets

## Naming Conventions

- **Folder names:** `snake_case`, descriptive, often suffixed with author/source name
- **Common prefixes:** `claude_code_*`, `cc_*`, `claude_cowork_*`, `autoresearch_*`
- **Flagged folders:** `__IMPORTANT` = high-value, `__SKIPPED` = incomplete, `__FAILED` = abandoned
- **Standalone notes:** Title Case, created at vault root (e.g., `Synthesis - Agent Teams.md`)
- **Project notes:** Use `README.md` inside the project folder

## Frontmatter Standards

Always include these properties when creating notes:

```yaml
---
description: Brief one-line summary
title: Display Title          # or use 'name' for skill-style files
date: YYYY-MM-DD
source: https://... or vault-journal or vault-synthesize etc.
---
```

- `description` is the most-used property in this vault (1,286 notes) — always include it
- Do NOT use tags — this vault uses properties as its organizational system (zero tags in use)

## Reading Vault Content

- Prefer **`gemini3_summary.txt`** over `transcript.txt` — summaries are structured and token-efficient
- Check **`README.md`** first for code projects
- Use `obsidian search:context query="..." limit=10` for contextual search across the vault
- Use multi-word queries — single keywords produce too many results across 10,918 files

## Linking Conventions

- Link to project folders: `[[folder_name/README]]` or `[[folder_name/gemini3_summary]]`
- Link to daily notes: `[[YYYY-MM-DD]]`
- Link to standalone notes: `[[Note Name]]`
- The vault has ~4,979 unresolved links — check that a file exists before creating a wikilink

## Obsidian CLI Usage

The vault name for targeting is `_claude_code`. Always use the `silent` flag when creating or modifying files programmatically:

```bash
obsidian create path="folder/note.md" content="..." silent
obsidian append path="folder/note.md" content="..."
```

For notes longer than ~2,000 characters, use the `Write` tool to write directly to disk instead of passing content via CLI argument (avoids shell argument length limits). Obsidian's file watcher picks up changes automatically.

**Obsidian must be running** for any CLI command that reads vault data (`search`, `daily:read`, `tags`, etc.) — the CLI communicates with the app via a local socket. Writing files directly to disk with the `Write` tool works without Obsidian open.

## Daily Note Format

Daily notes are at vault root: `YYYY-MM-DD.md`. When appending entries, use this format:

```markdown
### HH:MM — Topic Title
Content here.
**Related:** [[project_folder/README]], [[Another Note]]
```

## Workflow Skills

These slash commands are available for Obsidian productivity workflows:

| Skill | Purpose |
|-------|---------|
| `/vault-journal` | Log research entries to today's daily note |
| `/vault-capture` | Save web content (URL) as a formatted vault note |
| `/vault-synthesize` | Search vault on a topic and produce a synthesis note |
| `/vault-moc` | Generate a categorized Map of Content index |
| `/vault-health` | Audit vault health and produce a cleanup report |
| `/vault-save` | Capture insights from current conversation into the vault |
