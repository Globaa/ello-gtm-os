# Workflow B: Lagos businesses hiring a customer service or order-taking rep

**Status:** COPY-APPROVED 2026-11-17 (structure 2026-11-13; E1 2026-11-13; E2, E3 2026-11-17) · Clay-readiness fixes applied 2026-11-17 · Clay build-guide fixes applied 2026-11-17 (both approved by Fredrick) · shared HubSpot keys with Workflow C added 2026-11-17 (approved by Fredrick; Clay build guide updated to match the same day)

Clay build guide: `clay-build/hiring-customer-service-rep-clay-build-guide.md` (from `/clay-build-plan`, 2026-11-17).

Built with `/build-workflow-spec` from `context/icp/icp.md`, `context/icp/personas.md`, `context/icp/signal-matrix.md` (signal U1) and `context/docs/gtm-system.md`. Facts for copy come only from `context/messaging/facts.md` (approved 2026-11-13).

**Clay-readiness fixes (2026-11-17):** a pre-build check found that most sources were not reachable from Clay and that `account_key`, email finding, the CAC gate and `unclear` handling were undefined. Fixes below change sources, gates and reads only. Mother intent, segments, routing cells, angles and copy are unchanged. Two-stage build added as Section 3a.

## 1. Mother Intent (Trigger) [ENGINE]
**A Lagos business posted a job in the last 30 days for someone to answer customers: customer service, customer care, order-taking, sales rep or front desk.**
- Catalog: Hiring specific roles (`knowledge-base/playbooks/workflow-anatomy.md`); signal U1 in `context/icp/signal-matrix.md`.
- A job post costs money and effort to fake. It shows the business is budgeting for the job Ello's agent does: answering questions and taking orders and bookings.

## 2. Inclusion / Exclusion [ICP]
**Include:**
| Box | Rule | Source |
|---|---|---|
| Geography | Job location contains "Lagos" | icp.md (Lagos first, hypothesis) |
| Segment | Employer classified as S1 retail and social commerce, S2 food and takeout, or S3 salons and beauty | signal-matrix.md |
| Size | 1 to 3 open customer-facing roles at the employer (count of crawled posts per `account_key`; undercounts businesses that also post elsewhere); Instagram followers under 50k if an account is found (rule to test; follower count imported from Stage 1, check skipped when no account) | icp.md (5-seat top plan), gtm-system.md step 5 |
| Registration | CAC-registered. **v1: review step, not a hard filter** (see Exclude). Strike-off check removed until a source exists | icp.md, gtm-system.md step 5 |
| Trigger | Title matches (whole words, case-insensitive) `customer (service\|care\|support)\|client service\|\border (taker\|taking\|processing\|officer\|handler)s?\b\|\b(online\|inside\|tele) ?sales\b\|\bsales rep(resentative)?s?\b\|receptionist\|front desk`, posted ≤ 30 days ago. `sales rep` titles continue only if the segment AI column passes | signal-matrix.md U1 |
| Recipient | The owner (economic buyer); if no owner name is found, the business ("{{business_name}} team") | personas.md, gtm-system.md step 7 |

**Exclude:**
| Who | Why | How it's read |
|---|---|---|
| Recruiters, staffing firms, call centres, outsourcers posting for clients | Not the buyer | Employer name matches `recruit\|staffing\|outsourc\|bpo\|human resource\|manpower\|consult`, OR AI column returns `recruiter` |
| "Confidential" or missing employer | No account to match | Employer empty or contains `confidential` |
| Employers outside S1 to S3 (banks, telcos, fintech, logistics, clinics) | Not ICP; clinics on hold | AI segment column returns `other` or `clinic` |
| Segment `unclear` | Cannot route to a cell | AI segment column returns `unclear`: goes to a review view, never to sending |
| More than 3 open customer-facing roles | Too big for Ello's plans | Clay lookup (multiple rows) on the jobs table by `account_key`, count > 3 |
| Competitors: Viqe Systems (Zana), CherryBrooks (CallAI), Obeks, NativeTalk, PressOne, Salescabal (Bumpa), Fresha | Competitors | Cleaned employer name on blocklist (formula) |
| Existing Ello users | Already customers | **Not active in v1.** Turns on when the Ello team provides a sign-up handle list (open question) |
| HubSpot customer, open deal, or contacted by any Ello workflow in last 90 days | Suppression; a business must never get Workflow B and Workflow C at once | HubSpot lookups on the shared company properties `maps_place_id` (first) and `instagram_handle` (when Stage 1 found one); `name\|lagos` keys without either property are looked up on `account_key` (Section 6, changed 2026-11-17) |
| Not found on CAC | Ello cannot verify them | **v1: review step.** Claygent column on rows that passed every free gate (about 1 Action per row plus AI cost), output `found \| not_found \| unclear`; `not_found` and `unclear` go to a review view; or check by hand before sending |

## 3. Sources [ENGINE]
| Purpose | Source | Stage and tool | Cost |
|---|---|---|---|
| Trigger (main) | Jobberman Lagos customer service category (https://www.jobberman.com/jobs/customer-service-support/lagos) plus each job page | Stage 1: `tools/crawl4ai`, weekly via n8n. Clay cannot read it (no API; HTTP returns raw HTML) | Free locally |
| Trigger (secondary) | LinkedIn Jobs, Lagos | Stage 1: treg `apify.linkedin.search.jobs`. Optional: check whether Clay's own job-post search covers Lagos (preview the count first) | $0.001 per call (treg, 2026-11-12) |
| Employer to account (`account_key`, Maps category, website) | Google Maps search by employer name + Lagos | Stage 1: treg `crawl4ai.google.serp.maps` | $0.00023 per call (treg, 2026-11-12) |
| Instagram account | Maps website field, or link on the job page | Stage 1: parse. Lookup by business name still OPEN | $0 |
| Instagram followers and public email | Profile | Stage 1: treg `tikhub.instagram.user.profile`, only when an account is found | $0.001 per call (treg, 2026-11-12) |
| Segment and recruiter classification | Job description + employer name (+ Maps category when imported) | Stage 2: Clay Use AI column, JSON output | About 1 Action per row plus AI cost |
| `{{role_title}}` cleaning | Job title as posted | Stage 2: Clay Use AI column (can share the segment column's prompt as a second output field) | Included in the segment column if shared |
| Registration review | CAC public search (website form, no API) | Stage 2: Claygent column, last paid gate before email finding; or by hand | About 1 Action per row plus AI cost |
| HubSpot suppression | HubSpot companies by `maps_place_id`, then `instagram_handle`, then `account_key` (shared properties, Section 6) | Stage 2: Clay HubSpot lookups, only on businesses with 1 to 3 open roles | Some Clay plans charge for CRM lookups: check the Actions counter on 1 row |
| Owner first name | Instagram bio, Maps owner replies (imported from Stage 1) | Stage 2: formula picks the first non-empty; else "{{business_name}} team" | Free |
| Email | Order: (a) email shown on the job post (Stage 1 crawl); (b) Claygent on the Maps website contact page; (c) Instagram public email (Stage 1). First found wins | Stage 2: formula + Claygent (b only runs when a and c are empty) + Clay email validation | About 1 Action per row for each paid step that runs |

Not built in v1: Meta Ad Library (signal 6) and Instagram captions (signal 5). See Section 4.

**Hiring inboxes (decided 2026-11-17):** an email from source (a) is often the CV inbox. Any email starting `hr@`, `career(s)@`, `job(s)@`, `recruit…@`, `cv@`, `hiring@` or `vacanc…@` is **held** in a review view and not sent until reviewed.

**Two test sizes:**
- **Build test:** 25 job posts, to prove every column works.
- **Email coverage test (before scaling):** import every post from the last 30 days, so at least 20 businesses pass every gate, and record how many get a valid email. Rule to test: if fewer than half do, the manual DM/WhatsApp queue becomes the main channel, and that needs a decision before launch.

## 3a. Build architecture [ENGINE]
Clay cannot reach Jobberman, treg or the CAC site directly, so the build has two stages.

**Stage 1, outside Clay (crawl4ai + treg, run weekly by n8n). NOT BUILT YET.** Owner: NEEDS INPUT. First step: a one-off `tools/crawl4ai` run on the Jobberman Lagos customer service category **and each job page** (the list page alone has no full description), plus the Maps and Instagram lookups, saved as the CSV below. Until then the first CSV can be filled by hand from real posts. Fill `maps_place_id` for as many rows as possible: the name-based `account_key` fallback misses when one business is written two ways. Output one row per job post with these columns: `employer_name`, `title`, `location`, `posted_date` (converted from "3 days ago" to `YYYY-MM-DD` using the crawl date), `description`, `job_url`, `job_post_email` (if shown), `maps_place_id`, `maps_category`, `maps_website`, `instagram_handle`, `instagram_followers`, `instagram_email`, `owner_name_hint`. Import into Clay by CSV or webhook.

**Stage 2, inside Clay, in this order:**
1. Free formulas: clean employer name, `account_key`, trigger title regex, Lagos check, 30-day check, recruiter regex, confidential check, competitor blocklist, role channel (signal 3).
2. Lookups: open-roles count by `account_key` (free; exclude > 3); HubSpot suppression (may cost Actions on some plans; check on 1 row).
3. Paid, on rows that passed 1 and 2: segment + `role_title` AI column (`unclear` to review).
4. Paid, on rows in S1 to S3: CAC Claygent review.
5. Paid, on rows that passed 4: email chain and validation.
6. Free: copy formula columns (`e1l1`, `e1l2`, `e2l1`, `e3l1`), verbatim from the approved copy.
7. Sequencer push: built last and not run (sequencer not chosen; blocks launch, not build).

## 4. Signals table [ENGINE]
| # | Signal | Role | How it's read | Output |
|---|---|---|---|---|
| # | Signal | Role | How it's read | Clay data source | Output |
|---|---|---|---|---|---|
| 1 | Customer-facing job post, Lagos, ≤ 30 days | trigger | Regex on title (Section 2 pattern) AND location contains "Lagos" AND `today - posted_date ≤ 30` | Formula on imported Stage 1 fields (`posted_date` already `YYYY-MM-DD`) | boolean |
| 2 | Employer segment | router | Use AI column on employer name + job description (+ Maps category when present). JSON output `{segment, evidence}`. Enum: `s1_retail`, `s2_food`, `s3_salon`, `recruiter`, `clinic`, `other`, `unclear`. `unclear` goes to a review view, never to a cell | AI column | enum (only `s1_retail`, `s2_food`, `s3_salon` continue) |
| 3 | Channels the role covers | router | Regex on job description, case-insensitive, phrases about the work only. Chat: `\b(whatsapp\|instagram) (orders?\|messages\|enquiries\|dms?\|chats?)\b\|\bdms?\b\|direct messages\|live chat\|online (orders\|enquiries)\|social media (enquiries\|messages\|pages)\|respond(ing)? to (messages\|chats)`. Calls: `answer(ing)? (calls\|the phone\|phone calls)\|phone (orders\|calls\|enquiries)\|(inbound\|incoming\|outbound) calls\|telephone (orders\|enquiries)\|call (customers\|clients)`. Chat only = `chat`; calls only = `calls`; both = `both`; neither = `not_stated`. Test on 50 real descriptions before the full run | Formula | enum: chat / calls / both / not_stated |
| 4 | Job title as posted | context-clause | Cleaned to a short lowercase plain role (see Section 6) | AI column (shared with signal 2) | text → `{{role_title}}` |
| 5 | Takes orders in Instagram DMs (stacks with N1) | context-clause | Instagram account found AND a caption in last 14 days matches `dm to order\|send a dm\|whatsapp to order\|price in dm` | **Not built in v1.** Needs treg (Stage 1); no approved copy line uses it | boolean |
| 6 | Running a Meta ad (stacks with U3) | context-clause | Advertiser matches employer AND an ad started ≤ 30 days ago | **Not built in v1.** Needs treg (Stage 1); no approved copy line uses it | boolean |

**Several posts from one business (2026-11-17):** signals 3 and 4 are read from the business's **newest** post (latest `posted_date`). Older posts only count towards the open-roles check.

**Review and override (2026-11-17):** a `unclear` segment or a CAC miss goes to a review view. A reviewer can bring the row back with two manual columns: `segment_override` (only `s1_retail`, `s2_food` or `s3_salon`; replaces the AI segment) and `cac_override` (checkbox; ticked when CAC registration is confirmed by hand). Rows a reviewer leaves untouched are never sent.

Why signal 3 was tightened (2026-11-17): the old pattern matched "send your CV to this phone number", "must own a smartphone" and "chatty personality", which would route leads to the wrong Email 1 line 2 and Email 3 line 1.

Count clusters: no routing read is a count. The open-roles count is an exclusion filter (Section 2).

Deliberately excluded reads:
- Posted salary: not used in routing or copy. Copy built on it would pitch replacing a hire the owner just decided to make, and would trigger the job-loss objection in personas.md. Stored for analysis only.
- Review or follower counts: size proxy only (Section 2), not a router.

Routing axes for Section 5: Segment (3) x Role channel (4) = 12 cells. Approved by Fredrick 2026-11-13.

## 5. Routing matrix with angles [ENGINE]
Approved by Fredrick 2026-11-13. Segment (3) x role channel (4) = 12 cells, exhaustive (every lead that passes Section 2 lands in one cell; segment `unclear` is excluded to a review view before routing, 2026-11-17), so no fallback cell. Recipient in every cell: the owner. Spine: the hire answers customers in working hours; Ello's link and agent answer when nobody can, alongside the hire, never instead of them. Inference stops at what they are doing ("adding someone to answer customers"), never that they are losing customers.

| Cell | Deterministic read | What it tells us (inferred situation) | The angle it earns |
|---|---|---|---|
| S1 x chat | `s1_retail` + channel `chat` | A seller who takes questions and orders by message is adding `{{role_title}}` to answer DMs | The hire answers DMs in their shift; an Ello link in the bio answers price and stock questions and takes the order when they're off, then hands over |
| S1 x calls | `s1_retail` + `calls` | A seller whose customers phone to ask or order is adding someone to pick up | Customers call the business handle from the browser instead of a personal number; the agent picks up when the hire is on another call |
| S1 x both | `s1_retail` + `both` | The role covers phone and DMs, so customer conversations sit across a phone and apps | One link for calls and chats, so the hire works from one place and the agent covers the gaps |
| S1 x not_stated | `s1_retail` + `not_stated` | A seller is adding `{{role_title}}`; how customers reach them is unknown | One link in the bio to call or chat; the agent answers when the team can't |
| S2 x chat | `s2_food` + `chat` | A food business taking orders by message is adding someone to handle them | In the rush, the agent takes routine chat orders from the menu and opening hours; the hire handles what needs a person |
| S2 x calls | `s2_food` + `calls` | A food business taking phone orders is adding someone to answer the line | "Take orders without taking every call": customers order by call or chat from one link; the agent answers the overflow |
| S2 x both | `s2_food` + `both` | Orders come by phone and message, and the role covers both | Phone and chat orders arrive in one place; the agent takes the routine ones at peak |
| S2 x not_stated | `s2_food` + `not_stated` | A food business is adding `{{role_title}}`; channel unknown | One link for orders by call or chat; the agent answers when the team is busy |
| S3 x chat | `s3_salon` + `chat` | A salon getting booking questions by DM is adding someone to answer them | "Turn "Are you available?" into a booking": the agent answers availability by chat and books; the hire handles the rest |
| S3 x calls | `s3_salon` + `calls` | A salon booking by phone is adding a receptionist or front desk role | Clients call the salon handle from the browser; the agent books when the front desk is with a client |
| S3 x both | `s3_salon` + `both` | Bookings come by phone and DM | One link for booking calls and chats; the hire sees the customer history in one place |
| S3 x not_stated | `s3_salon` + `not_stated` | A salon is adding `{{role_title}}`; channel unknown | One link where clients call or chat to book; the agent answers between appointments |

Branch coverage: `{{role_title}}` and `{{business_name}}` exist in every cell. Context clauses (signals 5, 6) may add one line when true; they never pick the cell.

## 6. Variables contract [ENGINE]
Approved 2026-11-13. Updated 2026-11-17 (Clay-readiness fixes).

| Variable | Source field | Exists in branches |
|---|---|---|
| `{{account_key}}` | Internal, not in copy. Google Maps place ID (Stage 1); if none, formula: cleaned employer name, lowercase, trimmed, + `\|lagos` (e.g. `glow studio\|lagos`). Used for the open-roles count, HubSpot suppression and the 90-day dedupe | All |
| `instagram_handle`, `maps_place_id` (HubSpot) | Internal. Shared HubSpot company properties that **both** Workflow B and Workflow C write on every business they contact (handle lowercase, no "@", from Stage 1 `instagram_handle`; place ID from Stage 1 `maps_place_id`; empty when unknown), alongside `account_key`. Added 2026-11-17 so B and C suppress each other | All |
| `{{first_name}}` | Owner first name: first non-empty of Instagram bio hint, Maps owner reply hint (Stage 1); if none, resolves to "{{business_name}} team" in Clay | All |
| `{{business_name}}` | Employer name from the job post, cleaned (no "Ltd", no location suffix) | All |
| `{{role_title}}` | Job title as posted, cleaned by the segment AI column to a short lowercase plain role ("order taker", "customer service rep", "receptionist"); no gender, salary or location. If it can't be cleaned, falls back to "customer service rep". Copy uses it after "a new" (E1) and "your new" (E2) | All |
| `{{email}}` | First found of: job post email, Claygent on Maps website, Instagram public email. Only sent when Clay validation returns valid and it isn't a hiring inbox (Section 3) | All that reach sending |
| `segment_override` | Internal, not in copy. Manual review column (Section 4) | Reviewed rows only |
| `cac_override` | Internal, not in copy. Manual review checkbox (Section 4) | Reviewed rows only |
| `{{e1l1}}` | Email 1 line 1, by segment (formula, verbatim) | All |
| `{{e1l2}}` | Email 1 line 2, by role channel (formula, verbatim) | All |
| `{{e2l1}}` | Email 2 line 1, by segment (formula, verbatim) | All |
| `{{e3l1}}` | Email 3 line 1, by role channel (formula, verbatim) | All |

Removed 2026-11-17 (not used by any approved copy): `{{area}}`, `{{dm_to_order}}`, `{{meta_ad}}`.

## 7. Cadence & channel [ENGINE]
Approved 2026-11-13: **email only for now.** LinkedIn added later once the share of owners with a LinkedIn profile is known.
- Periodicity: always-on; new job posts pulled weekly (posts ≤ 30 days old) by the Stage 1 n8n run.
- Dedupe: one sequence per business per 90 days **across all Ello workflows** (HubSpot `maps_place_id`, `instagram_handle`, `account_key`). Every business pushed to the sequencer is written to HubSpot with all three properties.
- Sequence: Email 1 day 0 (new thread); Email 2 day 3 (same thread); Email 3 day 8 (same thread). In the sequencer, set the waits between steps as **3 days, then 5 days** (most sequencers count the wait from the previous step, so don't enter 8).
- No email found: manual DM/WhatsApp queue (open decision, gtm-system.md). Becomes the main channel if the 20-row email coverage test (Section 3) comes in under half.
- Sequencer: **not chosen** (open decision). The Clay push step can be built but not run; this blocks launch, not the build.

---

## Copy (email only)
Written with `/campaign-copy` from Sections 5-6 and `context/messaging/facts.md` only. Sender: Fredrick, Ello.

### Email 1 (day 0, new thread) · APPROVED 2026-11-13
Approved after a persona review and human test.

**Subject:** `{your new hire|the new hire|about your new hire}`

```
{{first_name}},

{{e1l1}}

{{e1l2}}

Ello gives you a handle: one link customers tap to {call or chat with you|call you or chat}, with no app to download.

When your new hire is {busy|tied up} or off for the day, they turn on our AI agent, and it answers routine questions from your catalogue and opening hours.

Want me to set up a handle so you can see it?

Fredrick
Ello
```

**`{{e1l1}}`: by segment (exhaustive, no fallback)**
| Segment | Line |
|---|---|
| `s1_retail` | You're hiring a new {{role_title}}, so my guess is more customers are asking about prices and stock. |
| `s2_food` | You're looking for a new {{role_title}}, so I'd guess orders are picking up. |
| `s3_salon` | With a new {{role_title}} on the way, more clients are probably asking to book. |

**`{{e1l2}}`: by role channel (exhaustive, no fallback)**
| Channel | Line |
|---|---|
| `chat` | The post says the job is mostly DMs and WhatsApp, and I imagine those keep coming after the shift ends. |
| `calls` | From the sound of the post it's mostly phone calls, and calls rarely come one at a time. |
| `both` | The post has them on calls and DMs, which is a lot of places to watch at once. |
| `not_stated` | Whoever you hire will cover their shift, but customers don't keep office hours. |

Facts used: "No app download for customers", "Turn the agent on when you need help", "uses your catalogue and opening hours", approved CTA. ~88 words.

### Email 2 (day 3, same thread) · APPROVED 2026-11-17
New value vs E1: the hire's side (handover with customer history). Subject: `Re:` E1 subject, unchanged.

```
{{first_name}},

{{e2l1}}

{The agent|Our agent} {can take|can handle} those and pass anything sensitive or complex to your new hire, along with the customer's history, so they {pick up where the agent left off|never start from scratch}.

{Their time goes to|That leaves them free for} the customers who {really|actually} need a person.

{Would that fit how you see the role?|Is that close to how you want the role to work?}

Fredrick
Ello
```

**`{{e2l1}}`: by segment (exhaustive, no fallback)**
| Segment | Line |
|---|---|
| `s1_retail` | I'd guess your new {{role_title}} will hear the same few questions most days: sizes, colours, delivery. |
| `s2_food` | At busy times, your new {{role_title}} will probably get the same orders and the same "are you open?" questions over and over. |
| `s3_salon` | My guess is your new {{role_title}} will hear "Are you available?" more than any other question. |

Facts used: "sensitive or complex questions passed to a person" (Variant B), "Customer history and handover" (Starter feature; no plan named in copy), salon vertical line. ~62 words.

### Email 3 (day 8, same thread) · APPROVED 2026-11-17
New value vs E1/E2: how easy it is to start (setup, no new phone, private number, where the link goes) and a polite close. Subject: `Re:` E1 subject, unchanged.

```
{{first_name}},

{{e3l1}}

Most businesses are set up in one sitting, and {there's no new phone or SIM: Ello runs in the browser|Ello runs in the browser, so there's no new phone or SIM to buy}.

{Worth setting one up for {{business_name}} before your new hire starts, or should I leave it here?|Should I set one up for {{business_name}} before your new hire starts, or leave it here?}

Fredrick
Ello
```

**`{{e3l1}}`: by role channel (exhaustive, no fallback)**
| Channel | Line |
|---|---|
| `chat` | The handle sits in your Instagram bio, so customers who already DM you can tap it to chat or call. |
| `calls` | Customers call your handle from their browser, without your personal phone number ever being exposed. |
| `both` | Customers call or chat from the same link, so your new hire works from one place instead of a phone and several apps. |
| `not_stated` | Your handle can go in your Instagram bio, on your Google profile, on a business card or in your ads, so customers always know where to reach you. |

Facts used: "Most businesses are set up in one sitting.", "No. Ello runs in the browser" (no new phone or SIM), "without your personal phone number ever being exposed", where the link goes; final-email CTA per the ladder. ~52 words.

### Sample: S3 x chat, fully assembled (as a prospect receives it)
Illustrative lead: owner "Ada", business "Glow Studio", role "receptionist".

**Email 1** · Subject: `your new hire`
> Ada,
>
> With a new receptionist on the way, more clients are probably asking to book.
>
> The post says the job is mostly DMs and WhatsApp, and I imagine those keep coming after the shift ends.
>
> Ello gives you a handle: one link customers tap to call or chat with you, with no app to download.
>
> When your new hire is busy or off for the day, they turn on our AI agent, and it answers routine questions from your catalogue and opening hours.
>
> Want me to set up a handle so you can see it?
>
> Fredrick
> Ello

**Email 2** (day 3) · Subject: `Re: your new hire`
> Ada,
>
> My guess is your new receptionist will hear "Are you available?" more than any other question.
>
> The agent can take those and pass anything sensitive or complex to your new hire, along with the customer's history, so they pick up where the agent left off.
>
> That leaves them free for the customers who actually need a person.
>
> Would that fit how you see the role?
>
> Fredrick
> Ello

**Email 3** (day 8) · Subject: `Re: your new hire`
> Ada,
>
> The handle sits in your Instagram bio, so customers who already DM you can tap it to chat or call.
>
> Most businesses are set up in one sitting, and there's no new phone or SIM: Ello runs in the browser.
>
> Worth setting one up for Glow Studio before your new hire starts, or should I leave it here?
>
> Fredrick
> Ello
