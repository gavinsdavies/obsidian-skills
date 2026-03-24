---
name: vault-sync-conflicts
description: Resolve Unison vault sync conflicts between WSL (obsidian-local) and Windows (obsidian) vault copies
---

# Vault Sync Conflict Resolution Skill

Resolves conflicts reported by `vault-sync` (Unison) between the two vault copies:

| Path | Role |
|------|------|
| `~/obsidian-local/AndenHjerne/` | **Primary** — WSL/Linux, git-tracked, where edits happen |
| `~/obsidian/AndenHjerne/` | **Windows sync copy** — Obsidian on Windows reads here |

## Process

For each conflicted file reported by Unison:

### 1. Diff both sides

```bash
diff ~/obsidian-local/AndenHjerne/<path> ~/obsidian/AndenHjerne/<path>
```

### 2. Determine the winner

- **WSL copy wins** if: it has more content, or contains edits just made in this session
- **Windows copy wins** if: it has more content, or has a newer timestamp with substantive changes
- The copy with **more content** is usually correct — a later timestamp on the other side typically means Obsidian auto-saved a less-complete version

If genuinely ambiguous, show the diff to the user and ask.

### 3. Copy winner → loser

```bash
cp ~/obsidian-local/AndenHjerne/<path> ~/obsidian/AndenHjerne/<path>
# or reverse if Windows won
```

### 4. Wait briefly, then reverse-copy to pick up the auto-updated timestamp

Obsidian on Windows **auto-updates the `updated` frontmatter timestamp** when it sees a file change. After copying WSL → Windows, wait a few seconds, then copy back:

```bash
sleep 3
cp ~/obsidian/AndenHjerne/<path> ~/obsidian-local/AndenHjerne/<path>
```

This ensures both copies have identical content including the correct `updated` timestamp, so the next sync sees no conflict.

> **Skip the reverse-copy** if Windows copy won (Obsidian already has the right timestamp on that side; just copy Windows → WSL directly).

### 5. Repeat for all conflicted files, then re-run sync

```bash
vault-sync
```

The re-sync should complete with 0 conflicts.

## Guidelines

- **Never** use the `updated` timestamp alone to decide the winner — Obsidian auto-saves frequently and can stamp a less-complete version with a newer time
- **Do** check word count / line count when content difference is subtle
- If both sides have unique content (genuine divergence), merge manually before copying
- The `created` field should never change — if it differs, something went wrong
