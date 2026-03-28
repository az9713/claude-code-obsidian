---
title: Claude Code + Obsidian Workflows
description: How six vault workflow skills were designed, what they do, how to use them, and what to expect — a quick-start guide for Obsidian users new to Claude Code integration.
date: 2026-03-27
source: vault-design-session
---

# Claude Code + Obsidian Workflows

> [!tip] Who this is for
> You have Obsidian installed and Claude Code running. You've heard they can work together but aren't sure where to start. This guide walks through six ready-to-use workflow skills, what they actually do, and the exact prompts to trigger them.

---

## How These Skills Were Designed

This vault (`_claude_code`) is a large research knowledge base — over 312 project folders capturing Claude Code tutorials, experiments, and tools. The challenge: all that content is hard to navigate, capture to, and search across manually.

The design process started by asking three questions:

1. **What does someone actually do every day** with a research vault? (Read, capture, log, navigate, clean up)
2. **What does Obsidian CLI make possible** that would otherwise be manual? (Search across all notes, append to daily notes, create files with frontmatter, audit link health)
3. **What does Claude add** that CLI alone can't do? (Understand content, generate structure, extract key insights, decide where to file things)

The result is six skills that sit at the intersection of those three answers — each one automating a real daily workflow rather than a hypothetical one.

---

## The Six Skills

### 1. `/vault-journal` — Daily Research Log

**What it does**

Every time you learn something about Claude Code, AI tooling, or anything relevant to your work, you can log it to today's daily note in one sentence. The skill handles the rest: it searches the vault for related project folders, constructs wikilinks to them, and appends a timestamped, structured entry.

**Why it matters for beginners**

Daily notes are one of Obsidian's most powerful features, but they're only useful if you actually write in them. This skill removes the friction — you don't need to know Obsidian's syntax, figure out where to put things, or manually search for related notes. Just tell Claude what you learned.

**How to invoke it**

```
I just learned that Claude Code agent teams use a shared message bus, not direct function calls between agents. Log this to my daily note.
```

```
Log that I spent time today setting up the obsidian-cli skill and it's working. Link to any relevant vault projects.
```

**What to expect**

Claude searches the vault for folders matching your topic, then appends an entry like this to today's `YYYY-MM-DD.md`:

```markdown
### 14:32 — Agent Teams Message Bus
Claude Code agent teams communicate via a shared message bus rather than direct function calls between agents — this enables decoupling and parallel execution.
**Related:** [[claude_code_agent_team_mark_kashef/gemini3_summary]], [[claude_code_agent_team_ai_jason/README]]
```

If today's daily note doesn't exist yet, it creates one with a header first.

---

### 2. `/vault-capture` — Save Web Content to Vault

**What it does**

Give it a URL. It fetches the page content using Defuddle (a tool that strips navigation, ads, and clutter from web pages), generates a structured summary with key takeaways, discovers related vault content to wikilink, and files everything into a new project folder following this vault's naming conventions.

**Why it matters for beginners**

Saving web content manually to Obsidian is tedious: copy URL, create folder, name the file, write frontmatter, paste content, format it, add links. This skill does all of that in one step and produces a note that looks like everything else in the vault.

**How to invoke it**

```
Save this to my vault: https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/overview
It's about Claude's tool use API.
```

```
Capture this article and file it: https://simonwillison.net/2025/Mar/11/claude-code/
```

**What to expect**

A new folder is created (e.g., `claude_tool_use_overview_anthropic/`) containing a `README.md` with:

- Frontmatter: `title`, `description`, `date`, `source` (the URL)
- A **Key Takeaways** section (Claude-generated bullet summary)
- The full extracted article content, cleaned up
- A **Related** section with wikilinks to relevant existing vault folders

A brief entry is also appended to today's daily note referencing the new folder.

> [!note] Defuddle requirement
> If you see an error about `defuddle`, run `npm install -g defuddle` once in your terminal to install it globally.

---

### 3. `/vault-synthesize` — Search & Synthesize Vault Knowledge

**What it does**

You give it a topic. It runs multiple search queries across all 10,000+ notes in the vault, reads the most relevant files (prioritizing pre-generated summaries over raw transcripts), and produces a new synthesis note that consolidates everything the vault knows about that topic — with citations back to source folders.

**Why it matters for beginners**

A vault full of individual project folders is only as useful as your ability to connect them. Synthesis is the hardest part of a knowledge base — normally it requires you to manually search, read, and write. This skill does that research work for you and produces a citable, navigable note you can actually use.

**How to invoke it**

```
What does this vault know about scheduled tasks and automation in Claude Code? Synthesize a note.
```

```
I want to understand all the different approaches to browser automation covered in this vault. Search and synthesize.
```

**What to expect**

A new note at the vault root, e.g., `Synthesis — Browser Automation.md`, containing:

- **Overview** — 2-3 paragraphs summarizing the collective knowledge
- **Key Insights** — numbered list of the most important findings, each citing its source folder with a wikilink
- **Patterns & Approaches** — how different sources agree or differ
- **Source Notes** — a table of all consulted folders with links
- **Gaps** — what the vault does *not* cover well on this topic (often the most useful section)

A reference entry is also appended to today's daily note.

---

### 4. `/vault-moc` — Map of Content Generator

**What it does**

Generates a categorized index note (called a "Map of Content" in Obsidian communities) that groups project folders by theme and links to them. Think of it as an auto-generated table of contents for a section of your vault.

**Why it matters for beginners**

With 312 folders, the vault's file explorer is overwhelming. A MOC gives you a curated entry point — a single note where everything is organized into readable categories with clickable links. It's the difference between having a library and having a library with a card catalog.

**How to invoke it**

```
Create a Map of Content for all the agent team and multi-agent projects in this vault.
```

```
Generate a focused MOC for everything related to skills, prompts, and slash commands.
```

```
Make a full vault index MOC — categorize all the project folders by theme.
```

**What to expect**

A new note, e.g., `MOC — Agent Teams.md`, with sections like:

```markdown
## Agent Teams & Orchestration
> Projects exploring multi-agent coordination, parallel execution, and task delegation.

- [[claude_code_agent_team_mark_kashef/README|Agent Teams — Mark Kashef]] — Comprehensive guide to building agent teams
- [[claude_code_agent_team_ai_jason/gemini3_summary|Agent Teams — AI Jason]] — Focus on dispatch patterns
- [[claude_opus_4.6_agent_team/README|Opus 4.6 Agent Team]] — Latest model agent team demo
...
```

`__IMPORTANT` folders get a highlighted section at the top. Each link uses friendly display names (no underscores).

---

### 5. `/vault-health` — Vault Health Audit

**What it does**

Runs a diagnostic scan of the vault: counts unresolved wikilinks, orphaned notes (no incoming links), dead-end notes (no outgoing links), checks folders for missing README or frontmatter, and audits property coverage. Produces a report note with a prioritized list of things to fix.

**Why it matters for beginners**

Obsidian's power comes from connected notes. But connections break over time — files get renamed, notes get created without frontmatter, links go nowhere. Without a health check, these problems accumulate silently. This skill surfaces them so you know exactly what to fix and in what order.

**How to invoke it**

```
Run a health audit on this vault and save the report.
```

```
What are the biggest structural problems in this vault right now?
```

**What to expect**

A report note, e.g., `Vault Health Report 2026-03-27.md`, containing:

| Metric | Count |
|--------|-------|
| Total files | ~10,918 |
| Unresolved links | 4,979 |
| Orphan notes | TBD |
| Dead-end notes | TBD |

Followed by:

- **Top unresolved links** with suggested fixes (create the note, rename an existing one, or remove the link)
- **Folders missing README** or summary files
- **Folders missing frontmatter** (no `description` property)
- **Prioritized action items** — what to tackle first, second, third

> [!tip] Baseline context
> This vault currently has ~4,979 unresolved links — many of these come from skill and CLAUDE.md files referencing paths that don't exist in the vault. The health report will flag these but note they're expected and low priority.

---

### 6. `/vault-save` — Capture Conversation Insights

**What it does**

At any point during a Claude Code conversation, you can tell it to save what you've just figured out. It extracts the key insight from the conversation, writes a structured note with context and details, cross-references related vault folders, and logs a pointer in today's daily note.

**Why it matters for beginners**

The most valuable knowledge is the stuff you figure out in the moment — a fix that worked, a pattern that clicked, a command combination that solved the problem. Without a capture step, that knowledge lives only in the conversation transcript (buried and hard to find later). This skill turns ephemeral conversations into permanent, searchable vault notes.

**How to invoke it**

```
Save the key insight from this conversation — that the Obsidian CLI is already installed on Windows via Obsidian.com and doesn't need a separate install step.
```

```
Save everything we figured out about how local .claude/skills/ files are loaded — it's important context for future sessions.
```

```
Save this conversation as a note about how the vault-* skills were designed and why.
```

**What to expect**

A new standalone note at the vault root, e.g., `Obsidian CLI Windows Installation.md`:

```markdown
---
description: Obsidian CLI is pre-installed on Windows via Obsidian.com — no separate install needed
title: Obsidian CLI Windows Installation
date: 2026-03-27
source: conversation
---

# Obsidian CLI Windows Installation

## Context
Investigating how to install the Obsidian CLI skill on Windows before using vault workflow skills.

## Key Insight
The Obsidian CLI is already bundled with Obsidian on Windows. `Obsidian.com` (a terminal redirector) is placed on PATH by the Obsidian installer alongside `Obsidian.exe`.

## Details
Run `where obsidian` — if Obsidian is installed, you'll see:
`C:\Users\<user>\AppData\Local\Programs\Obsidian\Obsidian.com`

Verify it works: `obsidian help`
The app must be running for data commands to work.

## Related
- [[.claude/skills/obsidian-cli/SKILL.md]]
```

A pointer is also added to today's daily note.

---

## Quick Start Sequence

If you're new to this setup, here's the recommended order to try these skills:

1. **Start with `/vault-save`** — save something from this very conversation. It's the fastest win and shows the system working end-to-end.

2. **Try `/vault-journal`** — log what you learned setting this up. You'll see the daily note get created and populated.

3. **Run `/vault-health`** — get a baseline picture of the vault's current state.

4. **Use `/vault-synthesize`** on a topic you've been curious about — this is where the vault starts feeling like it has a memory.

5. **Generate a `/vault-moc`** for a category you use often — this becomes your navigation hub.

6. **Start using `/vault-capture`** every time you find a useful article or doc — your vault grows itself.

---

## Does Obsidian Need to Be Open?

Yes — the Obsidian app **must be running** for any CLI command that reads vault data (`obsidian search`, `obsidian daily:read`, `obsidian tags`, etc.). The CLI talks to the running Obsidian process via a local socket.

However, the `Write` tool writing `.md` files directly to disk works **without** Obsidian open — Obsidian's file watcher picks up the changes next time it launches. So `vault-capture` and `vault-synthesize` (which use `Write` for long notes) will create the files either way, but the `obsidian daily:append` step at the end of those skills requires Obsidian to be running.

> [!tip] Rule of thumb
> Open Obsidian before running any `vault-*` skill. The app is lightweight — keeping it open in the background is the simplest approach.

---

## How Local Skills Are Loaded

> [!info] For curious users
> These skills live in `.claude/skills/` inside this vault. They are **not** globally installed — they load automatically because Claude Code reads `.claude/` from the working directory when it starts. This means the skills are only available when Claude Code is opened from this vault folder.
>
> The `Skill` tool (used to invoke globally-installed skills) won't find them by name. To manually use one, read its `SKILL.md` directly and Claude will follow its instructions. This is a current limitation of how local vs. global skills are resolved.

---

## Related

- [[.claude/skills/vault-journal/SKILL.md]]
- [[.claude/skills/vault-capture/SKILL.md]]
- [[.claude/skills/vault-synthesize/SKILL.md]]
- [[.claude/skills/vault-moc/SKILL.md]]
- [[.claude/skills/vault-health/SKILL.md]]
- [[.claude/skills/vault-save/SKILL.md]]
- [[CLAUDE.md]]
