---
description: Find up to three relevant contacts per company, prioritizing your existing LinkedIn connections, and save referral or cold-outreach routes in the tracker
argument-hint: "<job key | application id | company> [max 1-3]"
---

# People and referral routes

Read the get-a-job skill, references/browser.md and outreach.md. Resolve the
job/application as outreach.md does; create a saved row and appDir if absent
without tailoring a resume merely to research people.

## Discover

1. Use the logged-in browser when available. Open LinkedIn's own people
   search for the company and relevant team; set the **1st-degree** filter
   through the UI. This is how to check the candidate's connections, not a
   public web search. Confirm current employer/team and visible connection
   degree; inspect at most 3 relevant profiles per company. For a referral,
   prefer an existing relevant employee connection, then team lead/recruiter.
2. If no suitable 1st-degree contact was found, remove that filter and look
   for 2nd/3rd-degree contacts in the team. Recommend a connection request when
   the profile exposes Connect; a cold DM only when Message is available and
   opens a usable composer. InMail is a separate paid route, not a free DM.
   A Message button alone is not evidence of a first-degree connection.
3. For small/recently funded startups, prioritize founder, hiring team or the
   appropriate engineering/product lead. Verify title and funding claims
   from current company/funding sources. Use only published recruitment or
   business contact email addresses. Never invent email patterns.
4. Retain at most 3 recommended people total per company, including existing
   saved contacts. Reuse stable contact IDs; don't reset message history on
   repeated searches. If browser/login access is missing, fall back to public
   company research and search links; record relationship as unknown and say
   the candidate's connections have not been checked. No results in a capped
   search means "none found in this search", not "you have no connections".

## Save and draft

Merge contacts into the app's outreach.json using outreach.md's schema and
these optional fields (the tracker displays them):

- relationship: first-degree | second-degree | third-degree | unknown
- route: referral | connect | dm | email | unavailable
- reason: why this person is relevant to this role
- checkedAt: ISO timestamp; source: evidence URL
- status: not-contacted | connection-pending | connected | sent | replied |
  declined | unknown
- lastContactedAt: ISO timestamp or null
- nextFollowupDate: YYYY-MM-DD or null

Draft a personalized connection note, referral DM, cold DM or email for each
appropriate route, using outreach.md and the actual JD. Save outreach.md too.
Every message has status `drafted` initially; existing sent messages keep
status/sentAt and their exact text. Do not imply a relationship you have not
verified. A connection request is only a recommendation until explicitly sent.

For inspection, open only LinkedIn search URLs constructed from the company
and profile links exposed by those results; record the canonical profile URL.
Respect browser.md pacing. Per campaign: at most 5 people searches and 15
profile detail pages per site; persist counters in data/search-plan.json.
If the cap is reached, leave remaining jobs marked pending contact research.
Never request a complete connection export or read session/cookie storage.
