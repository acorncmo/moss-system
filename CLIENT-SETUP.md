# Client-Side MOSS Instance Setup

Instructions for setting up MOSS on each client laptop/profile.

---

## BILT & Bettercomp (GitHub Sync)

These clients use the shared GitHub repo. Setup is simple: clone the repo.

### Initial Setup

```bash
# Navigate to where you want the MOSS folder
cd ~/Documents

# Clone the repo
git clone https://github.com/acorncmo/moss-system.git

# Or if you already have it, just pull
cd moss-system
git pull origin main
```

### Daily Workflow

**Start of session:**
```bash
cd ~/Documents/moss-system
git pull origin main
```

**End of session:**
```bash
git add Clients/BILT/action-items.md Clients/BILT/comms-log.md Clients/BILT/status.md
git commit -m "Update BILT - [brief description]"
git push origin main
```

(Replace `BILT` with `Bettercomp` as needed)

### Working with Claude

When working on the BILT or Bettercomp laptop, tell Claude:

```
I'm working on BILT. My MOSS folder is at ~/Documents/moss-system/Clients/BILT/

[Then give your request, e.g.:]
Here are my meeting notes from today's call with Tom Cameron:
[paste notes]

Please extract action items and add them to action-items.md
```

---

## Curinos (Manual Sync - Isolated Environment)

Curinos has a secure environment that can't connect to GitHub. You'll maintain a standalone MOSS folder there.

### Initial Setup

Create this folder structure on the Curinos system:

```
MOSS-Curinos/
├── 00-Inbox/                    # Drop transcripts and raw notes here
├── 01-Strategy/
├── 02-Meetings/
│   ├── Weekly-Syncs/
│   ├── Stakeholder-Meetings/
│   └── Monthly-Reviews/
├── 03-Reports/
│   ├── Weekly/
│   └── Monthly/
├── 04-Templates/
├── 05-Projects/
├── 06-Resources/
│   └── Competitive-Intel/
├── 07-Working/
│   ├── Current-Week/
│   └── Current-Month/
├── action-items.md
├── comms-log.md
└── CLAUDE-CONTEXT.md
```

**Quick setup command** (run in terminal on Curinos system):

```bash
mkdir -p ~/Documents/MOSS-Curinos/{00-Inbox,01-Strategy,02-Meetings/{Weekly-Syncs,Stakeholder-Meetings,Monthly-Reviews},03-Reports/{Weekly,Monthly},04-Templates,05-Projects,06-Resources/Competitive-Intel,07-Working/{Current-Week,Current-Month}}
```

### Copy Templates

You already have rich templates in your Curinos folder on the hub. Copy these files to the Curinos instance:

**From hub (`moss-system/Clients/Curinos/`) → To Curinos instance (`MOSS-Curinos/`):**

| Source File | Destination |
|-------------|-------------|
| `04-Templates/*` | `04-Templates/` |
| `action-items.md` | `action-items.md` |
| `comms-log.md` | `comms-log.md` |
| `How-to-Work-with-Claude.md` | Root folder |
| `README.md` | Root folder |

### Create CLAUDE-CONTEXT.md

Create this file in the root of MOSS-Curinos to give Claude context when you start a session:

```markdown
# Claude Context - Curinos

## About This Engagement
- **Client:** Curinos (curinos.com)
- **Role:** Fractional CMO
- **Duration:** January - May 2026
- **Primary Project:** Curinos One launch preparation

## Key Stakeholders
- Mike Jiwani - SVP Product & Pricing Strategy (owns marketing, primary contact)
- Olly Downs - CTO
- Sarah Welch - GM, Consumer Banking

## Current Priorities
1. Product marketing for Curinos One platform launch
2. ABM program development
3. Sales enablement (battle cards, competitive positioning)

## About Curinos
Data and analytics for financial institutions. Key products:
- Retail Deposit Optimizer
- Commercial Analyzer
- Digital Banking Analyzer
- Curinos One (upcoming unified platform)

## How to Work Here
- All files stay in this folder (secure environment)
- At end of session: copy action-items.md and comms-log.md to Steven's hub manually
- Templates are in 04-Templates/
- Drop transcripts in 00-Inbox/
```

### Syncing to Hub (Manual Process)

When you finish a work session on Curinos:

1. **Export the sync files:**
   - Copy `action-items.md`
   - Copy `comms-log.md`
   - (Use whatever secure method you have: email to yourself, USB, secure transfer, etc.)

2. **On your hub laptop:**
   - Paste into `moss-system/Clients/Curinos/`
   - Overwrite the existing files

3. **Commit to repo (optional):**
   ```bash
   git add Clients/Curinos/action-items.md Clients/Curinos/comms-log.md
   git commit -m "Sync Curinos action items and comms log"
   git push origin main
   ```

---

## Working with Claude on Client Systems

### Starting a Session

Always give Claude context at the start:

```
I'm working on [CLIENT NAME].
My MOSS folder is at [PATH].

Read CLAUDE-CONTEXT.md (or status.md) to get oriented.

Today I need to: [your task]
```

### Common Tasks

**After a meeting:**
```
Just finished a meeting with [person] about [topic].

Here are my notes:
[paste notes or transcript]

Please:
1. Format into meeting notes (save to 02-Meetings/)
2. Extract action items → add to action-items.md
3. Log this meeting in comms-log.md
```

**Processing a transcript:**
```
I have a transcript in 00-Inbox/[filename].

Please:
1. Summarize the key points
2. Extract action items → add to action-items.md
3. Move the formatted notes to 02-Meetings/
```

**Weekly review:**
```
Read my action-items.md and comms-log.md.

Give me:
1. Summary of open items by owner
2. Items completed this week
3. Overdue items
4. Suggested priorities for next week
```

---

## Folder Purposes Quick Reference

| Folder | Purpose |
|--------|---------|
| `00-Inbox/` | Drop zone for transcripts, raw notes |
| `01-Strategy/` | Plans, OKRs, strategic docs |
| `02-Meetings/` | Formatted meeting notes |
| `03-Reports/` | Weekly and monthly status reports |
| `04-Templates/` | Reusable templates (don't edit originals) |
| `05-Projects/` | Project-specific folders |
| `06-Resources/` | Research, competitive intel |
| `07-Working/` | Active scratch space |
| `action-items.md` | Master task list (SYNC FILE) |
| `comms-log.md` | Communication record (SYNC FILE) |
