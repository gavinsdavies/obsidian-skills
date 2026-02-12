# Meeting Cleanup Skill

**Name**: `meeting-cleanup`
**Description**: Clean up rough meeting notes, extract action items, and create tasks

## What This Skill Does

When invoked on a meeting note, this skill will:

1. **Read the meeting note** (user should provide path or it should be clear from context)
2. **Reorganize discussion notes** into clear, logical sections with headers
3. **Extract action items** from discussion and create properly formatted tasks
4. **Write a concise summary** (2-3 sentences) of the meeting
5. **Ask clarifying questions** if task ownership, due dates, or priorities are unclear

## Task Creation Guidelines

When creating tasks from action items:

- **Due dates**:
  - Immediate/urgent items: within 2-3 days
  - Regular follow-ups: 1-2 weeks
  - Long-term items: 1 month+
  - Ask user if unclear

- **Tags**: Based on project context
  - `#research/dune/csc` for CSC-related
  - `#research/dune` for general DUNE
  - `#research/nova` for NOvA
  - `#olemiss/gpc` for department items
  - Add specific tags: `#software/Phlex`, `#software/mach3`, etc.

- **Priority markers**: Use when mentioned
  - `⏫` for high priority
  - `🔼` for medium-high
  - `🔽` for low priority

- **Task format**:
  ```
  - [ ] task description 📅 YYYY-MM-DD #task #category #subcategory
  ```

- **Location**: Add tasks to the meeting note's Action Items section
  - Also note: "Action items from this meeting are tracked in [[Project Page#Action Items]]"

## Meeting Note Structure

After cleanup, meeting notes should have:

```markdown
---
frontmatter (don't modify)
---

# Meeting Title

**Date**: [[YYYY-MM-DD-Day]]
**Attendees**: [[Person 1]], [[Person 2]], ...

---

## [Topic Section 1]

[Organized notes with context and clear hierarchy]

### [Subsection if needed]

---

## [Topic Section 2]

---

## Action Items

Action items from this meeting are tracked in [[Project Page#Action Items & Follow-ups]].

**Immediate:**
- [ ] task 1 📅 YYYY-MM-DD #task #tags
- [ ] task 2 📅 YYYY-MM-DD #task #tags

**[Timeframe 2]:**
- [ ] task 3 📅 YYYY-MM-DD #task #tags

---

## Summary

[2-3 sentence summary covering: main topics discussed, key decisions made, important deadlines/milestones mentioned, and open questions]
```

## What NOT to Do

- Don't modify frontmatter (created, updated, tags, etc.)
- Don't delete important details from rough notes — reorganize them
- Don't create tasks for things already completed or just informational
- Don't guess at task owners if unclear — ask the user
- Don't add tasks for external people's responsibilities (unless user needs to follow up)

## Clarifying Questions to Ask

If any of these are unclear, ask the user:

- "Who should own this action item?" (if not clear from context)
- "What's the deadline for [task]?" (if urgency unclear)
- "Should this be tracked in [Project Page] or somewhere else?"
- "Is [item] an action or just informational?"

## Example Usage

User: `/meeting-cleanup` (while viewing a meeting note)
User: `clean up meetings/csc-leads-2026-02-12.md`
User: `meeting-cleanup @meetings/nova-collab-2026-01-15.md`

## Tips for Best Results

- Run this skill right after a meeting while context is fresh
- Have the meeting note open or provide the path
- If user says "add tasks to [Project]", create them there instead of meeting note
- Group related action items by timeframe or topic
- Use bullet points and clear hierarchy (##, ###)
- Bold important names, dates, or decisions
