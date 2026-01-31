# MOSS Workflow Guide

> **Marketing Operating System as a Service**
> Hub-and-spoke architecture for managing multiple clients across secure environments.

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    MOSS HUB (Your Laptop)                   │
│                                                             │
│   Dashboard.md ← Aggregated view of all clients             │
│   ├── Clients/BILT/action-items.md      ↔ GitHub sync       │
│   ├── Clients/Bettercomp/action-items.md ↔ GitHub sync      │
│   └── Clients/Curinos/action-items.md   ← Manual copy       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
        ↑                    ↑                    ↑
        │ git push/pull      │ git push/pull      │ manual
        ↓                    ↓                    ↓
┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│ BILT Instance │  │  Bettercomp   │  │    Curinos    │
│ (Client Env)  │  │   Instance    │  │   Instance    │
│               │  │ (Client Env)  │  │ (Client Env)  │
│ - Transcripts │  │ - Transcripts │  │ - Transcripts │
│ - Source docs │  │ - Source docs │  │ - Source docs │
│ - Local work  │  │ - Local work  │  │ - Local work  │
└───────────────┘  └───────────────┘  └───────────────┘
```

---

## Sync Methods by Client

| Client | Method | Repository | Notes |
|--------|--------|------------|-------|
| BILT Inc. | GitHub | `acorncmo/moss-system` | Full folder sync |
| Bettercomp | GitHub | `acorncmo/moss-system` | Full folder sync |
| Curinos | Manual | N/A | Copy action-items.md & comms-log.md only |

---

## Daily Workflows

### Starting Your Day (Hub)

1. **Sync latest changes:**
   ```bash
   cd /path/to/moss-system
   git pull origin main
   ```

2. **Check the dashboard:**
   Ask Claude to refresh Dashboard.md with current data from all clients.

3. **Review priorities:**
   Look at active items across all three clients.

---

### Working on a Client (Client Instance)

When you're on a client laptop/profile:

1. **Sync first** (BILT/Bettercomp only):
   ```bash
   cd /path/to/moss-system
   git pull origin main
   ```

2. **Do your work:**
   - Drop transcripts in `00-Inbox/` (if you add this folder)
   - Create meeting notes in `02-Meetings/`
   - Work on deliverables in `05-Projects/`

3. **Extract action items:**
   Ask Claude to read your transcript/notes and update `action-items.md`

4. **Log communications:**
   Ask Claude to add entries to `comms-log.md`

5. **Push changes** (BILT/Bettercomp only):
   ```bash
   git add -A
   git commit -m "Update [Client] - [brief description]"
   git push origin main
   ```

---

### Ending Your Day (Hub)

1. **Sync from clients:**
   ```bash
   git pull origin main
   ```

2. **For Curinos (manual):**
   - Copy `action-items.md` and `comms-log.md` from Curinos instance
   - Paste into `Clients/Curinos/` in the hub

3. **Update dashboard:**
   Ask Claude to refresh the Dashboard.md

---

## Meeting Workflow

### Before a Meeting

```
I have a meeting with [person] at [client] about [topic].
Prep me with:
1. Recent action items related to this
2. Last comms log entry with them
3. Questions I should ask
```

### After a Meeting

```
Meeting with [person] about [topic].

Notes:
[paste your notes or transcript]

Please:
1. Format into meeting notes
2. Extract action items → add to action-items.md
3. Add entry to comms-log.md
4. Tell me the top 3 follow-ups
```

---

## File Reference

### Tracking Files (Sync-Friendly)

| File | Purpose | Sync Behavior |
|------|---------|---------------|
| `action-items.md` | Task tracking with owners/dates | Append-only, merge-friendly |
| `comms-log.md` | Record of client touchpoints | Append-only, merge-friendly |
| `status.md` | Engagement overview | Replace (single editor) |

### Folder Structure (Per Client)

```
Clients/[ClientName]/
├── 01-Strategy/      # Plans, OKRs, frameworks
├── 02-Meetings/      # Meeting notes and agendas
├── 03-Reports/       # Weekly, monthly, quarterly reports
├── 04-Templates/     # Reusable templates
├── 05-Projects/      # Active deliverables
├── 06-Resources/     # Research, competitive intel
├── 07-Working/       # Scratch space, drafts
├── action-items.md   # ← SYNC FILE
├── comms-log.md      # ← SYNC FILE
└── status.md         # ← SYNC FILE
```

---

## Git Commands Quick Reference

**Daily sync (start of day):**
```bash
git pull origin main
```

**Commit changes (end of work session):**
```bash
git add Clients/[ClientName]/action-items.md Clients/[ClientName]/comms-log.md
git commit -m "Update [Client] action items and comms log"
git push origin main
```

**If you have conflicts:**
```bash
# See what's conflicting
git status

# For action-items.md conflicts (both added items):
# Open the file, keep both sets of items, remove conflict markers
git add [file]
git commit -m "Merge action items"
```

---

## Prompts for Claude

### Refresh Dashboard
```
Read Dashboard.md and all client action-items.md and status.md files.
Update Dashboard.md with:
- Current active item counts
- This week's priorities (items due this week)
- Recent activity from comms logs (last 7 days)
```

### Weekly Review
```
Read all three clients' action-items.md and comms-log.md files.
Give me a summary:
1. Total active items per client
2. Items completed this week
3. Overdue items
4. Key decisions made
5. What needs my attention next week
```

### Add Action Item
```
Add to [Client] action-items.md:
- [ ] **[today's date]** [Description] | Owner: [name] | Due: [date] | Source: [meeting/email]
```

### Log Communication
```
Add to [Client] comms-log.md:
### [today's date] | [Meeting/Email/Call] | [Participants]
**Summary:** [one line]
**Decisions:** [list]
**Follow-ups:** [list with checkboxes]
```

---

## Troubleshooting

### "I can't push to the repo"
- Check you have git credentials configured
- Verify you have write access to `acorncmo/moss-system`

### "Merge conflict in action-items.md"
- Both you and another session added items
- Open the file, keep all items from both sections, remove the `<<<<` markers
- Commit the resolved file

### "Curinos data is stale"
- This is expected - Curinos uses manual sync
- Copy the files when you have access to that environment

### "Dashboard is out of date"
- Ask Claude to refresh it
- The dashboard doesn't auto-update; it's refreshed on request

---

## Adding a New Client

1. Create the folder structure:
   ```bash
   mkdir -p Clients/[NewClient]/{01-Strategy,02-Meetings,03-Reports,04-Templates,05-Projects,06-Resources,07-Working}
   ```

2. Copy tracking templates:
   ```bash
   cp Clients/BILT/action-items.md Clients/[NewClient]/
   cp Clients/BILT/comms-log.md Clients/[NewClient]/
   cp Clients/BILT/status.md Clients/[NewClient]/
   ```

3. Update the client name in each file

4. Add to Dashboard.md in the Active Clients table

5. Determine sync method (GitHub or manual) and document in status.md
