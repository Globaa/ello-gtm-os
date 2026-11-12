# Ello Outbound GTM System

Status: DRAFT, 2026-11-13. Built from `context/icp/icp.md`, `context/icp/personas.md`, `context/icp/signal-matrix.md` and the tool list in `CLAUDE.md`. Nothing here is tested: Ello has no customers yet and no signal has been run. Numbers marked "rule to test" are starting settings, not findings. INFERRED = not supported by research.

## The shape of the system

```
1 Context OS ──► 2 Signals ──► 3 Find companies ──► 4 Enrich ──► 5 ICP fit
                                                                    │
      ┌─────────────────────────────────────────────────────────────┘
      ▼
6 Route by segment ──► 7 Find people ──► 8 Find + verify email ──► 9 Sequence
                                                                    │
      ┌─────────────────────────────────────────────────────────────┘
      ▼
10 Replies ──► 11 HubSpot CRM ──► 12 Weekly loop ──► back to 1
```

## The constraint that shapes everything
Ello sells to Nigerian micro businesses of 1 to 5 people, led by Instagram retail sellers and food businesses [icp]. Most have no website, no company domain and no LinkedIn page (INFERRED [signal-matrix]). Three consequences run through every step:
1. **The account key is the Instagram handle or Google Maps place ID, not a domain.** HubSpot and Clay both default to domain-based dedupe; that has to be overridden (steps 3, 11).
2. **Email and LinkedIn coverage will be low** (INFERRED). The system is designed for email and LinkedIn as asked, but step 8 routes accounts with neither to a manual Instagram DM / WhatsApp queue. Measure coverage in the first run before committing.
3. **The buyer is the owner.** One contact per account by default.

## Tools
| Tool | Role | Source |
|---|---|---|
| Claude Code + this repo | Context OS, specs, copy, weekly review | CLAUDE.md |
| treg | Instagram, TikTok, Google Maps, Meta Ad Library, LinkedIn Jobs data | CLAUDE.md; endpoints and prices in signal-matrix.md |
| `tools/crawl4ai` | Jobberman and Nigeria Property Centre scrapes | CLAUDE.md |
| Clay (+ `tools/rex`) | Account and people tables, enrichment, email waterfall, AI columns | CLAUDE.md |
| n8n | Scheduling, glue between treg, Clay, sequencer and HubSpot | CLAUDE.md |
| HubSpot | CRM of record | CLAUDE.md |
| Sequencer (email) | OPEN DECISION. Not in CLAUDE.md Tools. PlusVibe or Instantly fit the installed `/campaign-qa` skill | .claude/skills |
| LinkedIn sending tool | OPEN DECISION. Not in CLAUDE.md Tools | n/a |
| Miro | Diagram of this system | CLAUDE.md |
| Notion | Publish finished docs for the team | CLAUDE.md |

---

## Step 1: Context OS
**What happens:** Every downstream step reads the same source of truth: ICP, personas, signal matrix, research, rules (cite sources, no invented numbers, no em dashes, ask before sending).
**Tool:** this repo, read by Claude Code. Copy runs only through `/build-workflow-spec` then `/campaign-copy`.

| Decision | Answer leads to |
|---|---|
| Is the segment's ICP and angle in `icp.md` / `signal-matrix.md`? | Yes: proceed. No: run `/signal-matrix refresh` first |
| Is there a STRUCTURE-APPROVED workflow spec for this signal? | Yes: proceed. No: `/build-workflow-spec`, then stop for approval |
| Do proof points exist (named customers, numbers)? | Yes: allowed in copy. No (today): copy uses Ello's own claims only, no customer names or stats [cu] |

## Step 2: Buying signals (always on)
**What happens:** Scheduled jobs detect the top signals from the matrix. The signal starts the workflow; the ICP filter comes after [workflow-anatomy].
**Tool:** n8n schedules treg calls and scrapes; raw hits land in a Clay "Signals" table.

| Signal | Source / tool | Cadence (rule to test) |
|---|---|---|
| N1 DM-to-order Instagram posts (S1) | treg Instagram hashtag posts | Daily |
| N2 Food "orders open" posts (S2) | Same build, food hashtags | Daily |
| U1 Hiring customer service / order-taking rep | crawl4ai on Jobberman; treg LinkedIn Jobs | Weekly |
| U3 New Meta ad in Nigeria (S1, S2) | treg Meta Ad Library | Weekly |
| U4 "Couldn't reach" Google reviews (S2, S3) | treg Google Maps search + reviews | Weekly |
| U5 New opening | Google Maps first-review date; #nowopen | Weekly |
| N3 Busy salons without booking link (S3) | treg Google Maps | Monthly |

| Decision | Answer leads to |
|---|---|
| Is the hit inside the signal window (14 days for posts, 30 for jobs and ads, 90 for reviews)? | Yes: keep. No: drop |
| Is it in Lagos (location tag, bio, Maps address, job location)? | Yes: keep. No: drop (Lagos-first hypothesis [icp]) |
| Did a treg job fail or return zero rows twice in a row? | Yes: alert in the weekly review; check the endpoint's success rate in `catalog_get`. No: continue |
| Does a daily run's projected treg spend exceed the budget? | Yes: pause and ask Fredrick. No: run |

## Step 3: Find companies (account resolution)
**What happens:** Turn each signal hit into one account record. A job post gives an employer name; an Instagram post gives a handle; a review gives a Maps place. Resolve them to the same account.
**Tool:** Clay "Accounts" table. Key = Instagram handle, else Google Maps place ID, else normalised business name + phone.

| Decision | Answer leads to |
|---|---|
| Does the account already exist (same handle, place ID or phone)? | Yes: attach the new signal to it (signal stacking). No: create it |
| Is it already in HubSpot as customer, open deal, or contacted in the last 90 days? | Customer or open deal: stop, notify owner of record. Contacted < 90 days: hold until suppression ends unless a new signal fires (rule to test). No: continue |
| Is it a known Ello user? | Yes: stop; send to CRM as existing user. No: continue. Needs an Ello sign-up export (OPEN DECISION) |
| Job post only, no handle found? | Search Instagram and Google Maps by employer name. Found: link. Not found: drop |

## Step 4: Enrichment
**What happens:** Add what the ICP check and the copy need. Keep to 2 to 4 secondary signals [workflow-anatomy].
**Tool:** Clay with treg calls via HTTP; Claude AI column for classification.

| Field | Source | Cost per record (treg, 2026-11-12) |
|---|---|---|
| Bio, follower count, category, public email/phone, website link | treg Instagram profile | $0.001 (contact info endpoint $0.00721 if needed) |
| TikTok presence | treg TikTok profile | $0.00064 |
| Maps listing: address, rating, review count, website field | treg Google Maps | $0.00023 |
| Recent reviews | treg Google Maps reviews | $0.00015 |
| Active Meta ads | treg Meta Ad Library | $0.00165 |
| Vertical (S1 to S5), "orders by DM?", "booking link?" | Claude AI column on bio + posts | Model cost |
| Competitor in use (Zana, Obeks, Fresha link) | Bio and website-field parse | $0 |

| Decision | Answer leads to |
|---|---|
| Profile private, deleted or no posts in 30 days? | Yes: drop. No: continue |
| AI vertical classification confidence low? | Yes: send to a manual review view (sample, not every row). No: continue |
| Projected Clay + treg cost per qualified account above target? | Yes: cut the most expensive enrichment first (`/calculate-costs`). No: continue |

## Step 5: ICP fit check
**What happens:** Score each account against `icp.md` layers and the not-a-fit list. Then run the CAC gate (N4).
**Tool:** Clay formula columns + Claude AI column; CAC public search via HTTP (INFERRED: per-name lookup, no bulk API known).

| Check | Pass | Fail leads to |
|---|---|---|
| Nigeria / Lagos | Yes | Drop |
| Vertical is S1, S2 or S3 | Yes | S4/S5: park in a "later" view. Clinics: drop |
| Size looks like 1 to 5 people (followers under 50k as a proxy, rule to test; no multi-branch chain) | Yes | Larger: drop (top plan has 5 seats [co]) |
| Sells by enquiry (DMs, calls, bookings) | Yes | Drop |
| Not a not-a-fit type (contact centre, needs SOC 2, youth-focused) | Yes | Drop |
| CAC: registered and not on a strike-off list | Registered | Unregistered or not found: hold in a "not ready" view; do not contact (Ello cannot verify them [co]). Re-check monthly |

| Decision | Answer leads to |
|---|---|
| Name match on CAC is fuzzy? | Manual check on a sample; if most S1 sellers fail, report it in the weekly review (S1 may shrink [signal-matrix]) |
| All checks pass? | Tier A (2+ signals) or Tier B (1 signal) and move to routing |

## Step 6: Routing by segment
**What happens:** Pick the angle cell from Matrix 1 and the signal-specific line from Matrix 2.
**Tool:** Clay routing column; values must match the variables contract in the workflow spec.

| Decision | Answer leads to |
|---|---|
| Segment? | S1: "Instagram bio as business desk" angle. S2: "Take orders without taking every call." S3: "Turn "Are you available?" into a booking." [signal-matrix] |
| Which signal is strongest (priority: U1 hiring > U3 new ad > N1/N2 post > U4 review > U5 opening)? (rule to test) | That signal writes the opening line. Others become secondary lines, max 2 |
| Is a competitor visible (Zana, Obeks, Fresha)? | Yes: comparison route, factual only. No: greenfield route |
| U4 review signal? | Never quote the complaint. Use the situation ("busy weekends") only [signal-matrix guardrail] |
| Persona? | Owner by default. Social runner only if the signal is U3 and a named marketer is found |

## Step 7: Finding people
**What happens:** Find the owner's name and role.
**Tool:** Instagram bio and posts (Claude AI column), Google Maps owner replies to reviews, Clay "Find People" (LinkedIn) by business name, CAC record where available.

| Decision | Answer leads to |
|---|---|
| Owner name found in bio, posts, Maps replies or CAC? | Yes: contact = owner. No: contact = the business account itself ("Hi {{business_name}} team") |
| LinkedIn profile found for that name + business? | Yes: add to LinkedIn track. No: email and manual tracks only |
| More than one person found? | Owner first. Second contact (rep or social runner) only for Tier A, max 2 per account (rule to test) |

## Step 8: Finding and verifying emails
**What happens:** Find a deliverable email for the contact.
**Tool:** Clay waterfall (template: `tools/rex/templates/waterfall-work-email.json`) for anyone with a domain; public email from Instagram business profile or website for everyone else; verification inside the Clay waterfall.

| Decision | Answer leads to |
|---|---|
| Business has a domain? | Yes: run the work-email waterfall. No: use the public profile email |
| Email is a free domain (gmail.com etc.)? | Allowed: it is how micro businesses work (INFERRED). Flag the address type for deliverability reporting |
| Verification result? | Valid: email track. Catch-all: email track, only on a separate low-volume inbox pool (rule to test). Invalid or unknown: no email |
| No valid email and no LinkedIn? | Manual queue: Instagram DM or WhatsApp, sent by a person with approval (OPEN DECISION: legality and Meta policy for cold WhatsApp not researched) |

## Step 9: The sequence (email and LinkedIn)
**What happens:** Load approved copy and contacts into the sending tools.
**Tool:** email sequencer and LinkedIn tool (both OPEN DECISIONS); copy from `/campaign-copy`; QA with `/campaign-qa`. Every launch needs Fredrick's approval (CLAUDE.md rule).

Starting cadence (rule to test, 4 email steps + 2 LinkedIn touches over about 14 days):

| Day | Channel | Step |
|---|---|---|
| 1 | LinkedIn | View profile (if found) |
| 1 | Email | Signal line + one angle + soft CTA |
| 3 | LinkedIn | Connection request, no pitch |
| 4 | Email | Short follow-up, one Ello capability for their segment |
| 8 | Email | Social proof slot: empty today, so use Ello's live demo or a 1-line "how it works" |
| 14 | Email | Break-up |

| Decision | Answer leads to |
|---|---|
| CTA? | "Claim your free handle" (Free plan, [co]) is the default ask. OPEN: the live demo page returned HTTP 500 on 2026-11-12 [co]; check it before linking |
| Inbox placement OK? | Run `/inbox-placement-test` before the first launch and monthly. Pool fails: pause that pool |
| Bounce rate over 3% on a batch (rule to test)? | Pause the workflow, check step 8 verification |
| Connection accepted? | Yes: one LinkedIn message 2 days later. No after 7 days: email only |
| Signed up to Ello mid-sequence? | Stop the sequence (needs Ello sign-up sync, OPEN DECISION) |

## Step 10: Reply handling
**What happens:** Classify every reply and act fast.
**Tool:** sequencer webhook to n8n; Claude classifies; HubSpot task for the human.

| Reply type | Leads to |
|---|---|
| Interested / question | HubSpot task same day; reply with the onboarding link or answer from `context/` facts only |
| "Already use WhatsApp / Meta / Zana" | Objection reply from personas.md; log the competitor in HubSpot |
| "I'd rather answer customers myself." | Reply with Ello's own answer: you stay in control, turn the agent on when you need it [pe] |
| Not now | Suppress 90 days; re-enter only on a new signal |
| Wrong person | Ask for the owner; update contact |
| Unsubscribe / angry | Suppress permanently, all channels |
| Out of office / auto-reply | Pause 7 days, resume |
| Signed up | Mark won-to-signup; hand to onboarding |

| Decision | Answer leads to |
|---|---|
| Classifier confidence low? | Human reads it before any reply |
| Reply needs a fact not in `context/`? | Ask the Ello team; never invent (CLAUDE.md rule) |

## Step 11: CRM (HubSpot)
**What happens:** One record per business, every touch logged, pipeline tied to Ello's own funnel.
**Tool:** HubSpot, synced from Clay and the sequencer via n8n.

| Object | Key | Notes |
|---|---|---|
| Company | Custom unique property `account_key` (Instagram handle or Maps place ID) | Domain is often missing, so HubSpot's domain dedupe cannot be relied on |
| Contact | Email, else Instagram handle | Owner flagged |
| Properties | segment, signal(s), signal date, tier, CAC status, competitor seen, workflow #, campaign | Needed for the weekly loop |
| Pipeline | Contacted → Replied → Signed up (Free) → Verified and live → Paid (Starter / Growth) | Signed up and later stages need Ello product data (OPEN DECISION) |

| Decision | Answer leads to |
|---|---|
| Does the record match an existing company by `account_key` or phone? | Yes: merge. No: create |
| Did a contact sign up but never get verified? | Task: help with the registration certificate; feeds the CAC-gate learning |
| Paid plan? | Closed-won; tag the signal and angle that sourced it |

## Step 12: Weekly feedback loop
**What happens:** Every Monday, results flow back into the Context OS.
**Tool:** n8n pulls a HubSpot + sequencer report; Claude Code reviews it with Fredrick.

Report per workflow: accounts found, % passing ICP, % passing CAC gate, email found %, bounce %, reply %, positive reply %, sign-ups, verified, paid, cost per qualified account.

| Decision | Answer leads to |
|---|---|
| A signal produced 0 positive replies after 2 iterations? | Kill or rework it; record why in `knowledge-base/learnings/` and mark it in Matrix 3 |
| A segment converts much better or worse than another? | Update tiers in `icp.md` and priorities in `signal-matrix.md` |
| Email found rate is very low? | Revisit step 8: make the manual DM/WhatsApp track primary for that segment (needs decision) |
| New objection seen 3+ times? | Add to `personas.md` with the buyer's real words (closes the "no buyer quotes" gap) |
| First named customer agrees to be referenced? | Add to `context/docs/research/customers.md` and unlock social proof in copy |
| Anything changed? | Log it in this week's `work-log/` and set next week's `weekly-priorities/` |

---

## Four-pillar check

| Pillar | Where the system covers it | Gaps |
|---|---|---|
| Clean data | One `account_key` per business (step 3), dedupe and suppression (3, 11), CAC gate (5), email verification (8), HubSpot merge rule (11) | Instagram handle to CAC name matching is fuzzy; no Ello sign-up sync yet, so existing users may get outreach |
| Always-on signals | n8n schedules every top signal daily, weekly or monthly (2); new signals re-open suppressed accounts (3, 10) | Jobberman scrape and Meta Ad Library coverage for Nigeria are untested; no alerting tool chosen beyond the weekly review |
| Composable workflows | About 4 builds (Instagram hashtags/profiles, jobs, Meta ads, Maps/reviews) serve all three segments; segment only changes keywords and the routing cell (2, 6); skills chain spec → copy → QA | Sequencer and LinkedIn tool not chosen, so steps 9 and 10 cannot be built yet |
| Tight feedback loops | Reply classes feed objections into personas (10, 12); weekly kill/keep per signal (12); learnings and tracker updated weekly | Pipeline stops at "Replied" until Ello product data (sign-up, verified, paid) reaches HubSpot |

## Open decisions for Fredrick and the Ello team
1. Email sequencer and LinkedIn tool (not in CLAUDE.md Tools).
2. Ello product data into HubSpot: sign-up, verified, paid. Without it, steps 3, 9 and 11 cannot exclude users or measure revenue.
3. Manual Instagram DM / WhatsApp track: allowed, and who sends? Rules for cold WhatsApp in Nigeria are not researched.
4. Budget per week for treg and Clay.
5. Live demo page (HTTP 500 on 2026-11-12) fixed before it is used as a CTA.
6. Rules to test: 90-day suppression, max 2 contacts per account, 3% bounce stop, followers-under-50k size proxy, signal priority order.
