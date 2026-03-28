---
name: vault-health
description: Audit vault health: unresolved links, orphan notes, dead-end files, missing frontmatter, structural gaps. Produces an actionable report note with prioritized fix suggestions. Use when wanting to clean up, organize, or assess the current state of the vault.
---

# Vault Health

Diagnostic audit of the vault. Runs CLI checks, samples folder structures, and produces an actionable health report.

## Workflow

1. **Run all diagnostics** (run these in parallel where possible):

   ```bash
   obsidian unresolved total
   obsidian unresolved counts verbose format=json
   obsidian orphans total
   obsidian deadends total
   obsidian properties sort=count counts
   obsidian tags sort=count counts
   obsidian files total
   obsidian folders total
   obsidian version
   ```

2. **Analyze top unresolved links:**
   - From the `unresolved counts verbose` output, take the top 20 by count
   - For each, run `obsidian search query="<link target>" limit=3` to check if a near-match exists (typo, case difference, missing folder)
   - Classify each as: **create it** / **rename existing** / **remove the link** / **ignore**

3. **Sample folder structural completeness** (pick 15-20 folders at random from different prefixes):
   For each sampled folder, check:
   - Does `README.md` or a summary `.md` file exist?
   - Does the main `.md` file have a `description` property?
   - Does it have at least one of: `transcript.txt`, `gemini3_summary.txt`, code files?

   Use `obsidian search query="<folder name>" limit=3` to check, or `Read` tool to check the file directly.

4. **Compose the health report** using the `Write` tool:

   ```markdown
   ---
   description: Vault health audit — YYYY-MM-DD
   title: Vault Health Report YYYY-MM-DD
   date: YYYY-MM-DD
   source: vault-health
   ---

   # Vault Health Report — YYYY-MM-DD

   ## Dashboard

   | Metric | Count | Status |
   |--------|-------|--------|
   | Total files | <n> | |
   | Total folders | <n> | |
   | Unresolved links | <n> | ⚠️ High / ✅ OK |
   | Orphan notes | <n> | |
   | Dead-end notes | <n> | |
   | Tags in use | <n> | |
   | Distinct properties | <n> | |

   ## Unresolved Links (<total>)

   Top unresolved wikilinks with suggested actions:

   | Link Target | Count | Suggestion |
   |-------------|-------|-----------|
   | `[[link]]` | n | Create note / Rename `existing.md` / Remove links |
   ...

   > **Note:** Many unresolved links in this vault come from cross-references in skill files and CLAUDE.md files — these are expected and low priority.

   ## Structural Issues

   ### Folders Missing README or Summary
   Based on sampling ~<n> folders:
   - [[folder_name]] — no README or summary file found
   ...

   ### Folders Missing Frontmatter
   - [[folder_name/README]] — missing `description` property
   ...

   ## Property Coverage

   Top properties in use:
   | Property | Count |
   |----------|-------|
   | description | <n> |
   | title | <n> |
   ...

   **Recommendation:** Standardize on `description`, `title`, `date`, `source` for all new notes.

   ## Tag Adoption

   Tags currently in use: **<n>** (vault relies on properties instead of tags — this is intentional).

   ## Prioritized Action Items

   1. **High priority:** <specific action with folder/file names>
   2. **Medium priority:** <action>
   3. **Low priority / optional:** <action>

   ## Next Steps
   - Run `/vault-moc` to create navigation indexes for high-value categories
   - Run `/vault-journal` to start logging daily notes
   ```

5. **Write the file:**
   ```
   Write tool → <vault-root>/Vault Health Report YYYY-MM-DD.md
   ```

6. **Report** the top 3 issues found and the file path.

## Tips

- The vault has ~4,979 known unresolved links from a previous scan — this is baseline, not a crisis
- Many unresolved links come from skill/CLAUDE.md files referencing paths that don't exist in this vault — note this in the report
- Focus structural sampling on `claude_code_*` and `autoresearch_*` folders — these are the core content
- The most actionable finding is usually folders with no description property — these are hardest to navigate
