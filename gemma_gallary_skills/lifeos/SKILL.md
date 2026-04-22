---
name: lifeos
description: Access Sumit's personal LifeOS vault. Use this for anything about his life, work, projects, finance, travel, decisions, or daily notes. Covers capturing thoughts, searching notes, logging decisions, meeting notes, and loading current context.
require-secret: true
---

# LifeOS Vault

## Instructions

Call the `run_js` tool with:
- **script name**: `index.html`
- **data**: A JSON string with an `action` field and action-specific fields below.

---

### Actions

#### 1. Load Life Snapshot (start of session)
When user asks for a briefing, overview, or "what's going on":
- `action`: `"get_brief"`

#### 2. Search Vault
When user wants to find notes, recall something, or look up information:
- `action`: `"search"`
- `query`: String — what to search for
- `top_k`: Number (Optional, default 5) — number of results

#### 3. Get Context on a Topic
When user asks about a specific area (career, finance, travel, Parallax, exit plan, etc.):
- `action`: `"context"`
- `topic`: String — the subject to pull context for

#### 4. Quick Capture to Inbox
When user wants to capture a thought, idea, or note without a clear home:
- `action`: `"capture"`
- `title`: String — short title
- `content`: String — body of the note
- `tags`: Array of strings (Optional) — e.g. `["idea", "side-project"]`

#### 5. Get Today's Note
When user asks about today, priorities for today, or wants to log the day:
- `action`: `"today"`

#### 6. Get This Week's Note
When user asks about this week or a specific week:
- `action`: `"week"`
- `week`: String (Optional) — ISO week like `"2026-W17"`, blank for current week

#### 7. Work Snapshot
When user asks for work context, current projects, or what's on at work:
- `action`: `"work"`

#### 8. Open Decisions
When user asks what decisions are pending or open:
- `action`: `"decisions"`

#### 9. Log a Completed Decision
When user has made a decision and wants to record it:
- `action`: `"log_decision"`
- `title`: String — what was decided
- `decision`: String — the decision made
- `context`: String (Optional) — background
- `stakes`: String (Optional) — `"low"`, `"medium"`, or `"high"` (default `"medium"`)

#### 10. Track an Open Decision
When user is wrestling with a choice and wants to file it:
- `action`: `"open_decision"`
- `title`: String — decision topic
- `context`: String — background and why it matters
- `options`: String (Optional) — options being considered
- `deadline`: String (Optional) — ISO date like `"2026-06-01"`

#### 11. Log a Meeting
When user describes a meeting they had or just finished:
- `action`: `"meeting"`
- `title`: String — meeting topic or name
- `attendees`: Array of strings — who was there (use `[]` if unknown)
- `notes`: String — what was discussed
- `action_items`: String — follow-ups and next actions
- `date`: String (Optional) — ISO date, default today

#### 12. Append to a File
When user wants to add something to an existing note (e.g., today's note, weekly note):
- `action`: `"append"`
- `path`: String — vault-relative path, e.g. `"02_journal/daily/2026-04-22.md"`
- `content`: String — text to append

#### 13. Read a File
When user asks to see a specific note:
- `action`: `"read"`
- `path`: String — vault-relative path

#### 14. Vault Stats
When user asks about the vault size or index:
- `action`: `"stats"`

---

### Rules

- **Inbox rule**: If unsure where a note belongs, use `capture` — it routes to the inbox for nightly processing. Do NOT guess a vault folder.
- **Paths**: Vault folders use numbered prefixes: `01_identity/`, `02_journal/`, `03_projects/`, `04_areas/`, `08_decisions/`, `09_meetings/`, `00_inbox/`, etc.
- **Today's daily note path**: `02_journal/daily/YYYY-MM-DD.md`
- **Weekly note path**: `02_journal/weekly/YYYY-Www.md`
- **Decisions**: Open decisions live in `08_decisions/open/`; decision log at `08_decisions/decision_log.md`
- **Meetings**: All meeting notes go to `09_meetings/`

### Sample Commands

- "Brief me" → `get_brief`
- "What are my current projects?" → `context` with topic `"projects"`
- "Search for Parallax" → `search` with query `"Parallax"`
- "Capture this idea: build a side project for call analytics" → `capture`
- "What's on today?" → `today`
- "What's my work focus this week?" → `work`
- "What decisions am I sitting on?" → `decisions`
- "I decided to go with option B for the infra migration — stakes high" → `log_decision`
- "Log my meeting with the risk team about compliance" → `meeting`
- "Append 'finished Parallax pipeline' to today's note" → `append`
- "What's my career direction?" → `context` with topic `"career"`
- "Show me my finance hub" → `read` with path `"04_areas/finance/finance_hub.md"`
