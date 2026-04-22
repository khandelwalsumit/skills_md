---
name: lifeos
description: Access Sumit's personal LifeOS Obsidian vault. Use when the user asks about their notes, journal, meetings, decisions, tasks, focus areas, weekly or daily priorities, or wants to add/update anything in their personal knowledge base.
metadata:
  require-secret: true
  require-secret-description: "Paste your full MCP URL, e.g. https://mcp.yoursite.com/mcp/yourname/yourtoken"
---

# LifeOS Personal Knowledge Base

You have access to a personal Obsidian vault via the `run_js` tool.

## How to call

Always invoke `run_js` with this JSON:

```json
{
  "tool": "<tool_name>",
  "args": { ... }
}
```

## Session start rule

**Always call `get_brief` first** at the start of every session before answering anything personal. For work context, call `get_work_snapshot` instead.

## Tools

### Read & Search
| tool | description | args |
|---|---|---|
| `get_brief` | Sumit's life snapshot. Call at session start. | `{}` |
| `get_work_snapshot` | Full work context: focus + this week + today. Use instead of 3 separate calls. | `{}` |
| `search_vault` | BM25 full-text search with ranked excerpts. | `{"query": "string", "top_k": 5}` |
| `get_context` | Semantic search + tag match for a topic. Broader than search_vault. | `{"topic": "string"}` |
| `get_file` | Read full content of a file by path. | `{"path": "relative/path.md"}` |
| `list_files` | List markdown files in vault or subfolder. | `{"folder": "optional/subfolder"}` |
| `list_folders` | List all folders in the vault. | `{}` |
| `search_by_tag` | Find files with a specific frontmatter tag. | `{"tag": "string"}` |
| `get_vault_stats` | File count, folder count, index size. | `{}` |

### Daily & Weekly
| tool | description | args |
|---|---|---|
| `get_today_note` | Get or create today's work note. | `{}` |
| `get_weekly_note` | Get or create weekly priorities note. | `{"week": "2026-W15"}` (blank = current) |
| `get_open_decisions` | List all active open decisions. | `{}` |

### Write & Update
| tool | description | args |
|---|---|---|
| `add_note` | Create a note anywhere in the vault. Route unknowns to `00_inbox`. | `{"folder": "path", "title": "string", "content": "markdown", "tags": []}` |
| `add_outside_work_note` | Log learning, side projects, personal interests → `00_inbox`. | `{"title": "string", "content": "string", "tags": []}` |
| `add_meeting_note` | Create structured meeting note in `09_meetings/`. | `{"title": "string", "attendees": [], "notes": "string", "action_items": "string", "meeting_date": ""}` |
| `list_recent_meetings` | List n most recent meeting notes. | `{"n": 5}` |
| `update_file` | Replace body of existing file. Preserves frontmatter. | `{"path": "string", "content": "markdown"}` |
| `append_to_file` | Append with timestamp separator. Good for logs. | `{"path": "string", "content": "string"}` |
| `update_section` | Replace content under a specific `## Heading`. | `{"path": "string", "section_header": "## Heading", "content": "string"}` |
| `update_focus_areas` | Update `01_identity/focus_areas.md`. | `{"content": "markdown"}` |
| `update_quick_facts` | Update a section in `quick_brief.md`. | `{"section": "string", "content": "string"}` |

### Decisions
| tool | description | args |
|---|---|---|
| `open_decision` | Create structured open decision in `08_decisions/open/`. | `{"title": "string", "context": "string", "options": "", "deadline": "YYYY-MM-DD"}` |
| `log_decision` | Append completed decision to decision log. stakes: low/medium/high | `{"title": "string", "decision": "string", "context": "", "stakes": "medium"}` |
| `close_decision` | Resolve open decision and archive the file. | `{"decision_filename": "File.md", "outcome": "string"}` |

### Vault Management
| tool | description | args |
|---|---|---|
| `move_file` | Move or rename a file. | `{"from_path": "string", "to_path": "string"}` |
| `delete_file` | Permanently delete a file. No recycle bin. | `{"path": "string"}` |
| `rebuild_index` | Rebuild full-text search index after bulk changes. | `{}` |

## Inbox rule

If unsure where a note belongs → always use `add_note` with `folder: "00_inbox"`. Never guess a destination folder.

## Tips

- Search before reading — use `search_vault` or `get_context` to find the path, then `get_file`.
- Present note content cleanly without raw markdown syntax.
- For decisions, always check `get_open_decisions` before creating a new one to avoid duplicates.