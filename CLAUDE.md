# Operating rules for this project

## Stay on free tiers until revenue starts
The business has no paying customers yet. Every connected tool/app must be run
on its free plan and kept within free-tier limits until real revenue starts
coming in. Do not upgrade or recommend upgrading any tool without the user's
explicit go-ahead. This applies in every session — new or resumed.

Known free-tier ceilings to respect:

| Tool | Free-tier limit | Notes |
|---|---|---|
| MailerLite | ~1,000 subscribers / ~12,000 emails per month | Pace list growth into "Cold Outreach Leads" group accordingly. |
| Calendly | 1 active event type, no group-kind events | Group event types (e.g. phone-call "Free 30 min Demo") fail to activate on free plan — use solo-type events only until upgraded. |
| Apollo.io | ~80 lead credits / ~160 direct-dial / ~5,000 AI credits / 0 export credits per cycle | Avoid bulk pulls and CSV exports (export credits are at zero). |
| Brevo | 300 emails/day, unlimited contacts | Use as backup/transactional only, not primary sender. |
| HubSpot | Free CRM; "marketing contacts" enrolled in marketing sends are capped | Use purely as CRM/analytics, not for sending marketing email. |
| Canva | 1 basic Brand Kit, limited premium elements on free plan | Build with free-tier assets only. |
| Gmail / Google Calendar / Google Drive | Standard free Google account limits | Generous at current volume, low risk. |
| GitHub | Free tier | No concern at current repo size. |
| Zapier | 100 tasks/month, single-step Zaps only on free plan | Enabled 2026-07-10 for LinkedIn posting only (`share` and `create_company_update` actions), per explicit user approval — awaiting user's LinkedIn OAuth connection before first use. Do not enable other apps on Zapier without separate approval. |
| n8n | Free if self-hosted; limited executions on n8n Cloud trial | Currently parked — do not enable until user asks. |
| HyperFrames (HeyGen) | Limited trial render credits | Not started yet; check limits before first use. |
| Zoom for Claude | N/A | Permanently excluded per user instruction — never connect/use. |
| Stripe | N/A | Payment processing is already automated directly on ourfrontdeskai.com (outside Claude's connected tools) — not a gap to fix, no connector needed here. |

When any automation step would exceed a free-tier cap, stop and flag it to the
user instead of silently upgrading, degrading functionality, or pushing them
to pay. Only revisit these limits when the user confirms revenue has started
and gives explicit approval to upgrade a specific tool.

## Weekly execution report
Every week, produce a report covering everything executed that week across
the automation system: leads sourced, CRM changes, campaigns/content drafted,
Calendly bookings, PR/dev activity, KPI snapshot vs. targets, and any
free-tier or blocker flags raised. The finished report must be saved to
Google Drive as a native Google Doc (via the Google Drive connector, not just
posted in-conversation), then the user is notified that it's ready for
checking. This is delivered via the "Weekly Automation Report" Routine
(self-bound, fires into the originating session). If asked to change cadence
or delivery method, update that Routine rather than relying on memory of
this instruction alone.

## Recurring automation Routines
Routines exist for well-defined, low-risk, already-proven steps only — per
the "automation execution model" rule, nothing client-facing or judgment-
requiring is run as an unattended Routine. Current Routines:
- **Weekly Automation Report** — weekly summary per above, saved to Google
  Drive as a Google Doc, user notified when ready.
- **Daily Ops Check** — daily snapshot of Apollo credit usage, MailerLite
  subscriber count/campaign status, Calendly bookings, and HubSpot CRM
  count, flagging anything approaching a free-tier cap.
Do not add an hourly Routine unless there is a specific, well-defined,
low-risk task that actually needs hourly cadence — an empty recurring job
is not automation.

**Exception (decided 2026-07-10):** the user explicitly overrode the
"nothing client-facing unattended" default for the 7-Day LinkedIn Editorial
Campaign — scheduled Routines may auto-post each day's LinkedIn content live,
without a per-post approval gate, per that explicit instruction. This is a
one-time override for that specific campaign, not a standing policy change —
future LinkedIn/social content batches should default back to the
approve-before-posting model unless the user explicitly overrides again.

## Subagents for the OurFrontDeskAI automation system
Specialized subagents live in `.claude/agents/` for delegating domain-specific
work within a session (lead sourcing, CRM ops, content drafting, reporting).
They do not run unattended on their own — a session or Routine invokes them.
See each file's frontmatter for scope and tool access.

## HubSpot / MailerLite split (decided 2026-07-10)
HubSpot (1,107+ contacts) is the system-of-record CRM and stays there —
**do not bulk-migrate its contacts into MailerLite.** MailerLite's free tier
caps around ~1,000 subscribers total, and HubSpot's contact count alone is
already close to that ceiling. Only new, purpose-built lists (e.g. the
"Investors & Sponsors" group, "Cold Outreach Leads") get imported into
MailerLite directly from their source, not copied wholesale from HubSpot.
If this changes, it needs an explicit new decision from the user, not an
assumption carried over from an old task.
