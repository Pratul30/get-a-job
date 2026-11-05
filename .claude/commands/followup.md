---
description: Draft follow-ups for applications quiet 10+ days, and sweep stale ones to no_response
argument-hint: "[company] (optional)"
---

# /followup

Drafts first; send only per SKILL.md rule 6. Load `.claude/skills/get-a-job/SKILL.md`, read
`references/outreach.md` section 5 and `references/writing-style.md`.

## 1. Find candidates

If statuses may be stale (the user is not sure who replied), run or suggest
`/gmail-sync` first so replies already received are not followed up.


```bash
node bin/gaj.js track list --json
```

Active = status in `applied | screening | interview`. Quiet time = today minus
the row's `lastActivityAt` (from `track list --json`; it is the last non-undone
`history[].at`). Never use `updatedAt`: editing a note or field must not reset
the clock.

- **Due**: active, last activity ≥ 10 days ago, and fewer than 2 follow-ups
  sent (count confirmed sent follow-ups, not history entries marked drafted).
- **Maxed**: 2 follow-ups already sent → no more drafts; list them.
- **Stale**: status `applied`, last activity ≥ 45 days ago.

If `$ARGUMENTS` names a company, limit to it.

Also inspect outreach.json for saved/drafted rows: a referral request or cold
message can be due even when no application was submitted. Use each message's
sentAt/nextFollowupDate and recipient, not just the row's activity clock. Skip
draft-only, unknown-send, replied or declined messages. Pending connection
requests are reminders to check acceptance, not permission to send another
request. Preserve original sent text/history; save each follow-up as its own
message. Count verified sent follow-ups per recipient, not drafted files.

## 2. Draft

For each due application:
- Open the row's `appDir` (submitted `resume.md`, `outcome.md`) for context.
  Do not re-derive the slug. `appDir` null → use the tracker row alone. No new
  claims beyond the submitted resume.
- Draft per `outreach.md` section 5 (#1 or #2). Save as
  `<appDir>/followup_<YYYY-MM-DD>.md` (no `appDir` → create
  `data/applications/<slug>/` and set it with
  `track update <id> --field appDir=data/applications/<slug>`) with channel and
  recipient (if known) at the top.
- Banned-word grep + em-dash count; fix hits.

## 3. Record

Save drafts with `status: drafted`; drafting is not sending and must not reset
sentAt or lastContactedAt. When asked to send one, follow send.md, then record
the confirmed send:

```bash
node bin/gaj.js track update <id> --note "followup #<n> sent <YYYY-MM-DD>" --next-action "await reply" --next-date <today+7>
```

## 4. Stale sweep

List stale applications (company, role, applied date, days quiet). Ask (one
AskUserQuestion, multi-select) which to close. For each confirmed:

```bash
node bin/gaj.js track update <id> --status no_response --note "closed after <N> days without reply"
```

Never change status without confirmation.

## 5. Summary

Table: company · role · days quiet · action (draft path / maxed / closed / skipped).
