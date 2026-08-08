# Operating rules for this project

## Stay on free tiers until revenue starts
The business has no paying customers yet. Every connected tool/app must be run
on its free plan and kept within free-tier limits until real revenue starts
coming in. Do not upgrade or recommend upgrading any tool without the user's
explicit go-ahead. This applies in every session — new or resumed.

Known free-tier ceilings to respect:

| Tool | Free-tier limit | Notes |
|---|---|---|
| MailerLite | ~1,000 subscribers / ~12,000 emails per month | **Suspended 2026-07-10**, still unresolved. Per user decision (2026-07-14), stop surfacing this as an active blocker in reports/updates — the leftover SableAssent draft campaign has been deleted, and the business has pivoted to Apollo + Brevo for active lead-gen/outreach in the meantime (see "Active lead-gen focus" below). Per further user decision (2026-08-02): stop actively checking MailerLite at all in Daily Ops Check / Daily Execution Routines while suspended — do not chase reconnecting the connector or flag its auth status; just skip the MailerLite portion silently and continue with the rest of the routine. Only revisit MailerLite (checking status or re-authorizing) once the user reports it's reinstated. |
| Calendly | 1 active event type, no group-kind events | Group event types (e.g. phone-call "Free 30 min Demo") fail to activate on free plan — use solo-type events only until upgraded. |
| Apollo.io | ~80 lead credits / ~160 direct-dial / ~5,000 AI credits / 0 export credits per cycle | Avoid bulk pulls and CSV exports (export credits are at zero). **Per user decision (2026-08-08): stop the daily Apollo ICP-match company-search check-in (the "this will consume 1 credit, do you want to proceed?" prompt) in Daily Ops Check entirely** — this endpoint has been gated (`API_INACCESSIBLE`) or otherwise unproductive every time it's been attempted since 2026-07-15, and re-asking daily is no longer worth it. Daily Ops Check should still report credit balances/usage, just skip the ICP-match search step. Only revisit if the user explicitly asks for lead sourcing via this endpoint again. |
| Brevo | 300 emails/day, unlimited contacts | **Temporary primary sender as of 2026-07-10** (MailerLite suspended, see above) — but FrontDeskAI-only. The account's only configured sender identity is registered as "SableAssent Coin Corporation" <OurFrontDeskai@gmail.com>; never send FrontDeskAI content under that display name — always override the sender `name` per-campaign to "Daryl Speaks \| FrontDesk AI" (same verified email, no re-verification needed). Never send SableAssent/crypto content through this account — mixing brands on one sender is what got MailerLite suspended. Revert to backup/transactional-only once MailerLite is restored. |
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
Calendly bookings, PR/dev activity, KPI snapshot vs. targets, email campaign
analytics (sent/delivered/bounces/opens/clicks/unsubscribes/complaints per
Brevo campaign, plus MailerLite account status), a rollup of that week's 5
Daily Execution Routine logs (see below), and any free-tier or blocker flags
raised. The finished report must be saved to Google Drive as a native Google
Doc (via the Google Drive connector, not just posted in-conversation), then
the user is notified that it's ready for checking. This is delivered via the
"Weekly Automation Report" Routine (self-bound, fires into the originating
session). If asked to change cadence or delivery method, update that Routine
rather than relying on memory of this instruction alone.

**Agent fallback (decided 2026-08-02):** the data-gathering step normally
delegates to the `weekly-reporting-agent` subagent. If that agent doesn't
return in a reasonable time (it appears stuck/lost, not just slow), do not
keep waiting indefinitely — either relaunch a fresh instance of the agent, or
just compile the report directly from data already gathered in the session
(daily ops checks, daily execution logs, direct tool calls) rather than
leaving the user without a report. Note in the report itself when this
fallback was used.

## Recurring automation Routines
Routines exist for well-defined, low-risk, already-proven steps only — per
the "automation execution model" rule, nothing client-facing or judgment-
requiring is run as an unattended Routine. Current Routines:
- **Weekly Automation Report** — weekly summary per above, saved to Google
  Drive as a Google Doc, user notified when ready.
- **Daily Ops Check** — daily snapshot of Apollo credit usage, Calendly
  bookings, and HubSpot CRM count, flagging anything approaching a free-tier
  cap. MailerLite is skipped while suspended (see free-tier ceilings table).
- **Daily Execution - Monday/Tuesday/Wednesday/Thursday/Friday** (added
  2026-07-19) — runs the department's "Daily Execution System" 50-tasks/week
  checklist (source: Google Doc linked in that day's task, mirrored in
  Section 10 of the execution plan). Per user decision: only the subset of
  each day's 10 tasks with a real connected data source is actually checked
  (outreach/campaign performance, automation status, KPI pulses); publish-
  type tasks (educational post, case study, tutorial, weekly update) are
  drafted only as Gmail drafts, never auto-posted; everything else (MRR,
  churn, billing, support tickets, avatar QA, competitor intel, etc.) is
  logged as "not automatable yet" rather than faked, since no billing/
  support/product-analytics connector exists and no live avatars or paying
  customers exist yet. Each day posts a brief "Daily Execution Log" entry
  (silent-unless-notable, same tone as Daily Ops Check) that the Weekly
  Automation Report Routine rolls up. Revisit the automatable/non-automatable
  split as more connectors/data sources come online.
Do not add an hourly Routine unless there is a specific, well-defined,
low-risk task that actually needs hourly cadence — an empty recurring job
is not automation.

**Weekly usage limit — resume, don't skip (decided 2026-08-03):** all of the
above Routines (Weekly Automation Report, Daily Ops Check, and each Daily
Execution weekday) must keep firing on their normal schedules. If a firing
hits the session's weekly usage limit (the tool-level cap that returns
"You've hit your weekly limit," distinct from any tool's free-tier cap),
that day's checklist is NOT to be logged as "not automatable" or silently
dropped — it is incomplete/blocked, and must be picked back up and finished
as soon as the usage limit resets, before or alongside whatever Routine
fires next. E.g. if Wednesday's Daily Execution check is blocked by the
usage limit, the moment the limit clears, go back and complete Wednesday's
actual checklist (real checks + drafts) rather than only moving forward to
Thursday's. Note in the relevant Daily Execution Log / Weekly Report when a
day's items were completed late due to this, so the gap stays visible
rather than silently backfilled as if nothing happened.

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

## Active lead-gen focus (decided 2026-07-14)
With MailerLite suspension unresolved, the priority shifted to actively finding
and reaching clients using the tools that already work: **Apollo.io** for
sourcing/enriching leads matching the ICP (local service businesses — salons,
clinics, and similar — evaluating the $97/$299/$549 plans), logged into
HubSpot; and **Brevo** for outreach email, sent from the FrontDeskAI-only
sender identity per the rule above. Respect Apollo's free-tier credit caps
(no bulk exports) and Brevo's 300/day send cap. Clean cold-outreach lists
(remove/exclude high-bounce addresses) before reusing them for a new send.

## Cold-outreach Gmail drafts — real-recipient rule RESCINDED (decided 2026-08-03)
The 2026-07-23 rule below is **stopped effective immediately**, per explicit
user instruction on 2026-08-03. Do NOT draft cold-outreach content (educational
posts, case studies, tutorials, or similar) directly to the 6 lead email
addresses anymore. Go back to drafting this content as self-addressed Gmail
drafts (placeholder/self, not sent to any real lead) for the user's own
review, exactly like every other Daily Execution draft-only item. If the user
wants real-recipient drafts resumed at some point, that requires a fresh
explicit instruction — do not infer it from context or from old session
history.

~~Superseded rule (was in effect 2026-07-23 through 2026-08-02):~~ content was
drafted as real Gmail drafts addressed directly to: contact@urbanbetty.com,
info@deeprootsatxsalon.com, mail@modernsalonandspa.com, customercare@oasalons.com,
feedback@stfhealth.com, info@rlpmg.com. This is kept here only as a historical
record — do not act on it.

## HubSpot / MailerLite split (decided 2026-07-10)
HubSpot (1,107+ contacts) is the system-of-record CRM and stays there —
**do not bulk-migrate its contacts into MailerLite.** MailerLite's free tier
caps around ~1,000 subscribers total, and HubSpot's contact count alone is
already close to that ceiling. Only new, purpose-built lists (e.g. the
"Investors & Sponsors" group, "Cold Outreach Leads") get imported into
MailerLite directly from their source, not copied wholesale from HubSpot.
If this changes, it needs an explicit new decision from the user, not an
assumption carried over from an old task.
