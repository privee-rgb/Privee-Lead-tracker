Got it — so you want context written about what we built, specifically for use within Google Antigravity (to drop in as a rules file, project context, or agent instruction). Here's a clean context doc:

---

## Project Context — The Privee Group Lead Tracker

**What this is**
A full-stack lead tracking web application for The Privee Group, a creative and marketing agency. The tool manages outbound Instagram DM prospecting, pipeline progression, and deal closing across two team members: Pri (Privee) and Mayank.

**Architecture**
- Frontend: single-file vanilla HTML/CSS/JS, deployable to Netlify or any static host
- Backend: Google Apps Script deployed as a Web App, acting as a REST-style endpoint
- Database: Google Sheets ("Leads" tab), used as the persistent data store
- No npm, no build step, no framework — intentionally lightweight for non-technical users

**How data flows**
On every page load or manual sync, the frontend POSTs `{ action: "getAll" }` to the Apps Script URL and renders the returned leads array. All writes (add, update, delete) POST to the same endpoint with the relevant action and lead payload. The Apps Script reads/writes rows in the Leads sheet by matching on a unique `id` field (timestamp-based UID).

**Data model — lead object**
```
id, brand, handle, industry, user, dateReached, stage,
poc, phone, email, website, requirements, notes,
callDatetime, calEventLink,
proposalDeadline, proposalSentDate, negNotes,
proposalWip, proposalSent, callCompleted, followingUp,
closedWon, closedLost, createdAt, updatedAt
```

**Pipeline stages (in order)**
Outreach → Responded → Discovery Call Scheduled → Discovery Call Done → Proposal In Progress → Proposal Sent → Negotiation → Closed Won / Closed Lost / Following Up

**Google Calendar integration**
When scheduling a discovery call, the frontend calls `https://api.anthropic.com/v1/messages` with an MCP server config pointing to `https://gcal.mcp.claude.com/mcp`, asking Claude to invoke `gcal_create_event` on the user's primary calendar. Falls back to a pre-filled Google Calendar URL if the API call fails.

**Key UX rules**
- Contact details (POC, phone, email, etc.) only appear after a lead moves past "Outreach" stage
- Proposal tracking section only appears from "Discovery Call Scheduled" onwards
- Weekly goal counter tracks leads with `dateReached` in the current Mon–Sun window, target 250/week combined
- Apps Script URL is stored in `localStorage` — each user pastes it once on first load
- All data is shared (written to the same Sheet), so both Pri and Mayank see the same leads in real time

**Files**
- `privee_lead_tracker.html` — entire frontend, self-contained
- `Code.gs` — Google Apps Script backend (paste into Extensions → Apps Script in the Google Sheet)
- `SETUP_GUIDE.md` — step-by-step deployment instructions