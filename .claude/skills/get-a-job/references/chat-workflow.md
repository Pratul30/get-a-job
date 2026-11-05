# Conversational job search

## Route intent, then do the work

Read the relevant files in `.claude/commands/` as procedures, binding the
user's natural-language criteria where they refer to `$ARGUMENTS`.

| User intent | Procedures |
|---|---|
| Get started, use my CV, change targets | onboard.md / preferences.md |
| Find jobs from different sites | discover.md, scrape.md, browse.md; email-alerts.md if connected |
| Rank jobs / explain fit | rank.md |
| Find people, connections, referrers | people.md, outreach.md |
| Recently funded startups, unadvertised roles | funded.md, cold-dm.md, people.md |
| Tailor my resume | tailor.md |
| Apply to this job | apply.md |
| Ask this person for a referral / send this DM or email | outreach.md if needed, then send.md |
| Show my tracker / follow-ups / replies | track.md / followup.md / gmail-sync.md |

Use ordinary language in responses, with company/role/person names and tracker
links instead of making the user learn keys, CLI commands or slash commands.
Resolve "this job" from context; ask only when multiple plausible jobs remain.

## First session in Desktop's Code tab

Use a local session with this repository as its working folder. Check Node
20+, dependencies, profile files and browser tools. Install missing project
dependencies with `npm install` when needed. Run `doctor --validate` after
onboarding; fix reported setup issues before a search. If the project MCP
server is not available, explain the one-time enable/restart step; do not claim
to have connected a browser. Use the browser instructions for login handover.

No profile: ask for a resume (attachment or `inbox/`) and essential gaps:
target roles/seniority, location/remote scope, work authorization, constraints.
Follow onboard.md to build the files; do not ask to replace an existing profile
when it is already usable. A friend's clone must use their own local profile,
not the original candidate's files. Gmail is optional: check for search/read
and send tools separately. Report missing access and continue other sources.

## Search campaign (e.g. 100 jobs + ranking + contacts)

1. Save `data/search-plan.json`: version 1, request, targetCount, criteria,
   startedAt, updatedAt, phase, sources (site, searches, detailsRead, outcome),
   selectedJobKeys, rankedJobKeys, trackedJobKeys, enrichedJobKeys, blockers.
   This is a progress journal, not a second jobs database. Update it after each
   batch. On resume, query the CLI and reconcile the journal with actual files.
2. Explain the scope briefly and start the localhost tracker early using
   track.md. Reuse an already-running tracker; never kill an unrelated process.
3. Discover company boards and scrape free sources first; use funded.md if
   requested, then logged-in browser sites for coverage. Apply preferences and
   eligibility. Query jobs before importing to skip duplicates. Explicit
   "find and add/create a tracker" authorizes local import without a second
   selection question. Browser pacing and per-site caps still apply across the
   entire campaign; do not restart runs to bypass them.
4. Work in batches (about 20 jobs) until the target of unique suitable jobs is
   reached, sources are exhausted, or site/access caps block further progress.
   Read full descriptions; deduplicate across sites by company, role, location
   and requisition id when present, not URL alone. Preserve the selected
   posting and avoid duplicate tracker rows. Do not weaken criteria to hit N.
5. Rank each selected job per rank.md, explicitly including imported/tracked
   and previously unranked jobs, not just status `new`. Missing JD → read the
   posting in the browser or request it; keep it pending with a reason, never
   invent a score. Persist each completed rank through jobs:set.
6. Create saved tracker rows through `track add --job-key ... --upsert` with
   company, role, URL, track, fit/verdict from the rank and an application
   directory. Skip failed gates. Save `job_posting.md` there. Use the same
   appDir when tailoring later. Saved means shortlisted, not applied. Set the
   job status to tracked after saving; preserve rank data.
7. If requested, run people.md and outreach.md for each selected job, with
   at most 3 recommended contacts per job/company (or the user's smaller
   limit). Research can span batches; show progress and preserve partial work.
   Reuse verified company contacts across roles, but personalize drafts to the
   job. Do not run full resume tailoring for 100 jobs just to create contacts.
8. For funded startups with no matching posted job, create a separate open
   application using cold-dm.md. Do not count these as verified open jobs.
9. Show the tracker URL, actual unique jobs found, ranked, pending JD,
   contact-enriched, connection checks blocked, and source shortfalls. A login
   on one site need not block research from other available sources. Pause
   browser actions on that site until the user logs in.

## Actions and follow-ups

When asked to apply to a named job, complete tailor.md then apply.md; attach
the tailored PDF. When asked to contact a named person, prepare the exact
message and recipient, and follow send.md. Show a review of the final item
before submission/sending; retain approval for that exact reviewed item and
do not ask again unless its recipient, text or attachment changes. General
discovery is not authorization to contact everyone in the tracker.

For each actual send, save message status, sentAt, nextFollowupDate and the
confirmed recipient in outreach.json; log it on the tracker timeline and set
the row's next action to the earliest outstanding follow-up. Keep a sent
connection request distinct from a referral DM, and both distinct from a
submitted application. If the sent state is unclear, mark unknown and inspect
before retrying. Never automatically resend. Follow-ups are recipient-specific;
check replies and existing sends before drafting. Refresh statuses when asked;
there is no always-on inbox monitor unless separately configured.
