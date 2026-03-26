---
name: weekly-review
description: Conduct a weekly review by reading journal entries, summarizing accomplishments, tracking habits, and identifying priorities for the week ahead
---

# Weekly Review Skill

Helps conduct a structured weekly review from vault journal entries and recent activity.

## Inputs

The user may specify:
- A week (e.g. "last week", "week of March 3", "this week") — default is the **previous Monday–Sunday**
- Whether to save the output to a note (default: display only, ask at end)

If today is Monday, default to the week just ended (previous Mon–Sun).
If mid-week, default to the current week so far.

---

## Process

### 1. Determine Date Range

Calculate the Monday–Sunday range for the target week. Today's date is available in the system context.

### 2. Find Journal Files

Journal entries follow the pattern:
```
Journal/YYYY/MM-MMMM/YYYY-MM-DD-dddd.md
```
Examples:
- `Journal/2026/03-March/2026-03-02-Monday.md`
- `Journal/2026/02-February/2026-02-28-Saturday.md`

Use Glob to find files matching the week's date range. Read each file that exists (not all days will have entries).

### 3. Find Meeting Notes

Search for meeting notes from the week. Meeting notes live in paths like:
- `1-Projects/Research/DUNE/meetings/`
- `1-Projects/Research/NOvA/Meetings/`
- `1-Projects/Research/Meetings/`
- `1-Projects/Research/EMPHATIC/meeting-notes/`

Use Grep to find notes with `date:` frontmatter matching the week's date range, or use Glob with date-based filenames.

### 4. Extract from Journal Entries

From each daily note, collect:

**Notes content** (`## 📝 Notes` section):
- Free-form notes, quotes, highlights — these are the raw work log
- Note any wikilinks to projects, people, or meetings

**Tasks added** (`## ➕ Added Today` section):
- List tasks added during the week
- Mark which are now complete (✅) vs still open

**Habit data** (frontmatter fields):
- `exercise:` (minutes)
- `danish:` (minutes)
- `weight:` (lbs)
- Collect all non-null values to summarize

**Meals** (optional, skip unless user asks)

### 5. Synthesize the Review

Produce a structured weekly review with these sections:

#### 🏆 Highlights & Accomplishments
Group notable work by project area (DUNE, NOvA, EMPHATIC, Teaching, Service, Personal). Pull from Notes sections and completed tasks. Lead with wins, not just activity.

#### 🗓️ Meetings & Key Discussions
List meetings attended with one-line summary of key outcome or decision. Link meeting notes with `[[wikilink]]`.

#### ✅ Tasks Completed
Tasks marked done (✅) during the week, grouped by project tag.

#### ⚠️ Open & Overdue
Tasks added this week that remain open. Flag any that are past due relative to today.

#### 💪 Habits Summary
| Habit | Mon | Tue | Wed | Thu | Fri | Sat | Sun | Total/Avg |
|-------|-----|-----|-----|-----|-----|-----|-----|-----------|
| Exercise (min) | | | | | | | | |
| Danish (min) | | | | | | | | |
| Weight (lbs) | | | | | | | | |

Show total for exercise and danish; show range/trend for weight.

#### 🔭 Looking Ahead
- Upcoming deadlines in the next 7–14 days (scan project notes for due dates if helpful)
- 3–5 priority focus areas for the coming week
- Any open questions or blockers to resolve

---

## Output

Display the review in the conversation.

After showing the review, ask:
> "Would you like me to save this as a note? If so, I can write it to `Journal/YYYY/WW-weekly-review.md` or append it to your Monday note."

If saving, use standard Obsidian frontmatter:
```yaml
---
type: weekly-review
created: YYYY-MM-DDTHH:MM
date: YYYY-MM-DD
week: YYYY-WNN
tags:
  - weekly-review
  - journal
---
```

---

## Guidelines

**DO:**
- Lead with accomplishments, not just activity
- Be concrete — name the actual work done, not vague summaries
- Flag genuine blockers and overdue items clearly
- Keep the habits table honest (blank = no data, not zero)
- Link to relevant notes with `[[wikilinks]]`

**DON'T:**
- Fabricate habit data — only report what's in frontmatter
- Include boilerplate from journal templates (dataview blocks, nav links, etc.)
- Make the review longer than needed — quality over completeness
- Modify any journal files

---

## Example Invocation

```
/weekly-review
/weekly-review last week
/weekly-review week of March 3
```
