---
name: meeting-cleanup
description: Clean up rough meeting notes, extract action items, create tasks, and write summary
---

# Meeting Cleanup Skill

This skill helps clean up rough meeting notes after a meeting.

## Process

Follow these steps:

1. **READ** the meeting note file (user should provide path or it should be clear from context)

2. **REORGANIZE** rough discussion notes into clear sections:
   - Use descriptive headers (##) for main topics
   - Use subsections (###) where helpful
   - Add context and clarity to bullet points
   - Maintain all important details
   - Use bold for important names, dates, decisions

3. **EXTRACT** action items and create tasks:
   - Add to 'Action Items' section in the meeting note
   - Use format: `- [ ] task 📅 YYYY-MM-DD #task #tags`
   - Set realistic due dates (ask if unclear):
     * Immediate: 2-3 days
     * Regular: 1-2 weeks
     * Long-term: 1 month+
   - Tag appropriately: #research/dune/csc, #research/nova, #olemiss/gpc, etc.
   - Add priority if mentioned: ⏫ 🔼 🔽
   - Group by timeframe (Immediate, This Week, This Month, etc.)
   - Add note: 'Action items tracked in [[Project Page#Action Items]]'

4. **WRITE** a summary (2-3 sentences):
   - Main topics discussed
   - Key decisions made
   - Important deadlines/milestones
   - Open questions

5. **ASK** clarifying questions if:
   - Task ownership unclear
   - Due date/urgency uncertain
   - Task vs informational unclear
   - Where to track tasks (which project page)

## Guidelines

**DON'T:**
- Modify frontmatter
- Delete important details
- Create tasks for completed items
- Guess at owners or dates
- Add tasks for external people's work (unless user needs to follow up)

After cleanup, show the user what you changed and ask if they want any adjustments.
