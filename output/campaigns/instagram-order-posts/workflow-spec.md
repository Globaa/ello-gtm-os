# Workflow C: Lagos sellers and food businesses taking orders by message on Instagram

**Status:** COPY-APPROVED 2026-11-17 (structure, angle fix, E1, E2, E3 and DM all 2026-11-17) · Clay-readiness fixes applied 2026-11-17 · Clay build-guide fixes applied 2026-11-17 (both approved by Fredrick) · pre-build check fixes applied 2026-11-20 (approved by Fredrick: HubSpot suppression rule, CAC check, Lagos regex, competitor handles, post dedupe, Maps match, coverage-test rule; trigger row covers posts on both hashtag sets) · Clay build guide updated to match 2026-11-20 · shared `domain` suppression key added 2026-11-25 (approved by Fredrick; **Clay build guide NOT yet updated to match**)

Clay build guide: `clay-build/instagram-order-posts-clay-build-guide.md` (from `/clay-build-plan`, 2026-11-17).

**Clay-readiness fixes (2026-11-17):** a pre-build check found 14 issues (3 blockers: Stage 1 not built, Workflows B and C couldn't suppress each other in HubSpot, manual DMs not logged). Fixes below change sources, gates, reads and the variables contract only. Mother intent, cells, angles and copy are unchanged.

Built with `/build-workflow-spec` from campaign ideas #1 (DM-to-order sellers) and #2 (Food orders-open posts) in `output/campaigns/campaign-strategy.md`, run as one shared build (signal-matrix.md: "N1 and N2 are the same build ... with different hashtags"). Sources: `context/icp/icp.md`, `context/icp/personas.md`, `context/icp/signal-matrix.md` (N1, N2, U2, N4), `context/docs/gtm-system.md`. Facts for copy come only from `context/messaging/facts.md` (approved 2026-11-13). Every section approved by Fredrick on 2026-11-17.

## 1. Mother Intent (Trigger) [ENGINE]
**A Lagos seller or food business posted on Instagram in the last 14 days telling customers to order by message ("DM to order", "WhatsApp to order", "price in DM", "orders open", "pre-order"), on a Lagos vendor or food hashtag.**
- Catalog: Website content mentions (`knowledge-base/playbooks/workflow-anatomy.md`), adapted from website pages to Instagram posts. Signals N1 (retail) and N2 (food) in `context/icp/signal-matrix.md`.
- The post is an operating instruction to customers, not a marketing claim: it shows orders arrive today as DMs or WhatsApp messages someone has to answer, which is the job Ello's link and agent do.
- Weaker as a buying reason than Workflow B's job post (a post costs nothing to make; a job post means budget). Compensates with volume, cheap and reliable data (H in signal-matrix.md), timing (14-day window) and fit with Ello's own line "Turn your Instagram bio into a business desk."

## 2. Inclusion / Exclusion [ICP]
**Include:**
| Box | Rule | Source |
|---|---|---|
| Geography | Found on a Lagos hashtag (below) AND bio or post location matches (case-insensitive) `lagos\|lekki\|ikeja\|yaba\|surulere\|ajah\|ikoyi\|victoria island\|festac\|gbagada\|ikorodu\|magodo\|ogba\|oshodi\|egbeda\|ojodu\|ilupeju\|isolo\|apapa\|sangotedo\|chevron`. Every row comes from a Lagos hashtag, so this regex is the only real location filter. `maryland` (also a US state) and `vi` (matches any standalone "vi") were removed 2026-11-20; a bio like "Maryland, Lagos" or "VI, Lagos" still passes on "lagos". Rule to test on the first pull: non-Lagos sellers use Lagos hashtags too; add missed areas to this list | icp.md (Lagos first, hypothesis) |
| Segment | Classified as S1 retail and social commerce or S2 food and takeout. S3 salons and everything else are out of this workflow | signal-matrix.md N1, N2 |
| Size | Under 50k Instagram followers (rule to test, same threshold as Workflow B). Followers are the only size proxy; headcount isn't visible. A **blank** follower count fails (the rule can't be checked) and goes to the `Dropped` view, so missing profiles in Stage 1 are visible | icp.md (1 to 5 people), gtm-system.md step 5 |
| Account | Business or creator account (if the profile data returns that flag) passes. Otherwise the Use AI column's `is_seller` field decides from the bio: `yes` passes, `no` is out, `unclear` goes to review. Whether treg's profile call returns the flag (`is_business`) is unconfirmed: check it in the Stage 1 test and record the result here. If it doesn't, every account relies on `is_seller` | INFERRED: Ello sells to businesses [co] |
| Registration | CAC-registered. v1: review step, not a hard filter (same as Workflow B). Matching an Instagram name to CAC is fuzzier than matching a job-post employer | icp.md, signal-matrix.md N4 |
| Trigger | A post in the last 14 days on a listed hashtag whose caption matches the keyword list **for the hashtag set it was found on** (S1 hashtags use S1 keywords, S2 hashtags use S2 keywords; table below), or either set's list if the post was found on both (Section 3). The AI segment (Section 4) only routes; it never picks the keyword list | signal-matrix.md N1, N2 |
| Recipient | The owner (economic buyer); if no owner name is found, "{{business_name}} team" | personas.md |

**Trigger definition per segment:**
| Segment | Starting hashtags | Caption keywords (whole words, case-insensitive) |
|---|---|---|
| S1 retail | #lagosvendor, #lagosboutique, #shopinlagos, #lagosthriftstore | `dm to order\|send a dm\|whatsapp to order\|price in (the )?dm\|dm for price` |
| S2 food | #smallchopsinlagos, #lagosbaker, #lagosfoodvendor | `orders? (are )?(now )?open\|pre-?orders?\|taking orders\|(today'?s\|this week'?s\|weekly) menu\|dm to order\|whatsapp to order` |

Hashtags are from signal-matrix.md; the first test pull decides which to keep. `dm for price`, `taking orders` and `weekly menu` are additions to test on real captions.

**Exclude:**
| Who | Why | How it's read |
|---|---|---|
| 50k or more followers | Too big for Ello's plans | Profile follower count |
| Salons, clinics, anything outside S1/S2 (fashion schools, influencers, course resellers, event planners) | Not this workflow's ICP | Segment returns `s3_salon`, `clinic`, `other`; `unclear` goes to review |
| Non-Lagos sellers | Lagos first | No Lagos or Lagos area in bio or location |
| Competitors' own accounts: Zana, CallAI, Obeks, NativeTalk, PressOne, Bumpa, Fresha | Competitors | Formula: **exact match** of `{{account_key}}` against a list of the competitors' Instagram handles. Never a substring match on names ("zana" would block unrelated sellers). Handles (checked 2026-11-20, each as linked from the company's own website): `cherrybrooksofficial` (CallAI; links to the parent CherryBrooks LLC's account, gocallai.ng), `pressone.africa` (pressone.africa), `getbumpa` (getbumpa.com), `fresha` (fresha.com). Zana, Obeks and NativeTalk: no Instagram linked on their sites (usezanaai.com, obeks.com.ng, nativetalk.io, homepage and contact page) and none found by web search; NEEDS INPUT. Low risk meanwhile: a software company's account is classified `other` by the segment AI and drops out anyway |
| Existing Ello users | Already customers | Not active in v1 (needs the sign-up handle list) |
| Contacted by any Ello workflow in the last 90 days (email or DM), HubSpot customer, or open deal | Suppression; a business must never get Workflow B and Workflow C at once | Three HubSpot lookups: first `instagram_handle`, then `maps_place_id` if Stage 1 matched one, then `domain` when known (Section 6). `domain` is the only key shared with Workflow B since 2026-11-25; with no domain, do the manual B/C cross-check (Section 7). A found company is suppressed when `lifecyclestage` = customer, OR `hs_num_open_deals` > 0, OR `ello_last_outreach_date` is within the last 90 days (rule added 2026-11-20; confirm the first two internal property names in HubSpot settings) |
| Not found on CAC | Ello can't verify them | v1 review step |

## 3. Sources [ENGINE]
Two stages, as in Workflow B: Clay can't call treg. **Stage 1** runs outside Clay (treg, run weekly by n8n) and outputs a CSV. **Stage 2** runs in Clay. Prices per call from treg, 2026-11-12 (signal-matrix.md); no new paid calls made.

| Purpose | Source | Stage and tool | Cost |
|---|---|---|---|
| Trigger: recent posts | Instagram hashtag posts for each Section 2 hashtag | Stage 1: treg `anyapi.instagram.hashtag.posts` (99.4% success; the routed treg endpoint was only 68%) | $0.002 per call |
| Caption keyword match, 14-day check | Caption and date from the call above | Stage 1 filter (profiles fetched only for matching posts), repeated as a Stage 2 formula | Free |
| Profile: followers, bio, business flag, category, public email, bio link | Instagram profile of each matching account | Stage 1: treg `tikhub.instagram.user.profile` (99.9% success) | $0.001 per account |
| Maps match: place ID, Maps category, website | Google Maps search by business name + Lagos | Stage 1: treg `crawl4ai.google.serp.maps`; kept only when the listing links to the same Instagram handle, OR the listing name matches the cleaned business name exactly AND the listing address is in Lagos; else empty | $0.00023 per call |
| Lagos check, personal number in bio (U2), competitor blocklist | Bio, post location, handle | Stage 2: Clay formulas | Free |
| Segment, item, order day, owner first name | Caption + bio + Instagram category (+ Maps category) | Stage 2: one Clay Use AI column, JSON output with enums | About 1 Action per row plus AI cost |
| HubSpot suppression | HubSpot company by `instagram_handle`, then `maps_place_id`, then `domain` (Section 6); suppression rule in Section 2 | Stage 2: Clay HubSpot lookups | Check the Actions counter on 1 row |
| Registration review | CAC public search | Stage 2: Claygent, output `found \| not_found \| unclear`, run only on rows that passed every free gate and the AI segment gate (`s1_retail` or `s2_food`); or by hand (v1 review step). **Untested:** before the build test, run it on 5 businesses already known to be CAC-registered; if Claygent can't read the CAC search results, do this check by hand | More per row than Use AI |
| Email | (a) public email on the Instagram profile (Stage 1); (b) Claygent on the **bio link first, then the Maps website** (the first one that qualifies), only when (a) is empty AND the link is not `wa.me`, `api.whatsapp.com` or `instagram.com` (those pages never hold an email). Then Clay email validation | Stage 1 + Stage 2 | About 1 Action per paid step that runs |
| DM sent log | The person sending ticks `dm_sent` and fills `dm_sent_date` in Clay | Stage 2: manual columns, then a Clay HubSpot "create or update company + log activity" step on ticked rows | Check the Actions counter on 1 row |

**One row per post:** the posts table dedupes on `post_url`. A post found on both an S1 and an S2 hashtag is kept once and passes the trigger if the caption matches **either** set's keyword list (added 2026-11-20).

**One row per account:** Stage 2 sends matching posts into a `Sellers` table keyed on the Instagram handle. When an account has more than one matching post in 14 days, the **newest** post sets `item`, `order_day` and where orders go; older posts are kept in the posts table for reference only.

**Stage 1 output (one row per matching post; derived from the sources above):** `hashtag` (every hashtag the post was found on, comma-separated, so a post on both sets keeps both), `post_url`, `post_date` (`YYYY-MM-DD`), `caption`, `post_location`, `instagram_handle`, `display_name`, `bio`, `followers`, `is_business`, `ig_category`, `public_email`, `bio_link`, `maps_place_id`, `maps_category`, `maps_website`. Empty when not found; never guessed.

**Stage 1 is NOT BUILT YET.** Owner: NEEDS INPUT. First step: a one-off treg pull on 2 hashtags (one S1, one S2), with the price checked before each call; this doubles as the volume test. Until Stage 1 exists, the first CSV can be filled by hand from real posts.

**Two tests before scaling (rules to test, no numbers yet):**
- **Volume:** one hashtag pull per segment; count matching posts and unique accounts per 14 days. Replaces the strategy's "High" and "Medium" bands. Also check that `anyapi.instagram.hashtag.posts` returns **recent** posts (not only "top" posts) and how many posts one call returns; if it needs paging, record the real cost per 14 days.
- **Email coverage:** among accounts that pass every gate, how many get a valid email. Decides how much of the send goes by DM (Section 7). Run it on **every matching post from the last 14 days on all 7 hashtags**, not on the 25-post build test, which gives too few sellers. **Decision rule (added 2026-11-20):** compare the DM-branch accounts per week with what the daily manual DM cap (Section 7) can send in a week. If the queue is bigger, decide before launch whether to raise the cap, add a sender, or drop the oldest posts from the DM queue.

**Build test:** 25 posts, to prove every Clay column works. Separate from the coverage test above.

## 4. Signals table [ENGINE]
| # | Signal | Role | How it's read | Clay data source | Output |
|---|---|---|---|---|---|
| 1 | Order-by-message post, Lagos hashtag, ≤ 14 days | trigger | Post on a Section 2 hashtag AND caption matches the keyword list for that hashtag set AND `today - post_date ≤ 14` | Formula on imported Stage 1 fields (`hashtag`, `caption`, `post_date`) | boolean |
| 2 | Business segment | router | Use AI column on caption + bio + Instagram category (+ Maps category), hashtag set as a hint. JSON `{segment, is_seller, item, order_day, owner_first_name, evidence}`. Segment enum: `s1_retail`, `s2_food`, `s3_salon`, `clinic`, `other`, `unclear`. `unclear` goes to review | AI column | enum (only `s1_retail`, `s2_food` continue) |
| 3 | Where orders go | router | Regex on caption, then bio. DM if caption matches `\bdms?\b\|send a dm\|direct message\|inbox (us\|me)\|price in (the )?dm`. WhatsApp if caption matches `whatsapp\|wa\.me`, or (caption names neither) bio has a `wa.me` / `api.whatsapp.com` link or the word WhatsApp. DM only = `dm`; WhatsApp only = `whatsapp`; both = `both`; neither = `not_stated` | Formula | enum: dm / whatsapp / both / not_stated |
| 4 | What the post is selling | context-clause | AI field `item`: short lowercase noun phrase from the caption ("ankara dresses", "small chops trays"); empty if none | AI column (same as 2) | text → `{{item}}` |
| 5 | Order day (food only) | context-clause | AI field `order_day`: a **day name only** ("Friday") if the caption states the day orders are for; empty otherwise, including when the caption gives only a date | AI column (same as 2) | text → `{{order_day}}` |
| 6 | Personal number in bio (U2) | context-clause | Regex on bio with spaces and dashes removed: `(\+?234\|0)[789][01]\d{8}`. Catches any mobile number, including a business line; accepted 2026-11-20 because most small sellers use one phone for both, so the approved "personal phone number" line stands | Formula | boolean |

All reads use the account's **newest** matching post (Section 3, "One row per account").

Count clusters: no routing read is a count.

"inbox us" / "inbox me" is the common Nigerian way to say "DM us": an addition, to test on the first pull.

**Review and override:** `unclear` segment or a CAC miss goes to a review view. A reviewer can bring a row back with `segment_override` (only `s1_retail` or `s2_food`) or `cac_override` (checkbox). Rows left untouched are never sent.

**Open decision (NEEDS INPUT, 2026-11-20):** Instagram trading names often don't match CAC names, so most rows may land in CAC review. The current rule makes a CAC miss block sending until a reviewer ticks `cac_override`. The alternative is to send and only flag the row. Decide after the CAC test (Section 3).

**Deliberately excluded reads:**
- Likes, comments, post count: popularity, not situation. Followers are a size filter only.
- Meta ads (U3): a separate workflow (#4); mixing it in makes this one depend on an Ad Library test.
- Hashtag source as a router: a hint to the AI only; food vendors post on #lagosvendor too.
- Personal number in bio as a router: context line only (strategy: "an extra line on #1/#2 rather than a standalone send"); a static attribute, not timing.

Routing axes: Segment (2: `s1_retail`, `s2_food`) x Where orders go (4: `dm`, `whatsapp`, `both`, `not_stated`) = 8 cells. Approved 2026-11-17.

## 5. Routing matrix with angles [ENGINE]
Approved 2026-11-17. 2 x 4 = 8 cells, exhaustive (every lead that passes Section 2 lands in one; `unclear` is excluded to review first), so no fallback cell. Recipient in every cell: the owner.

**Spine:** the owner answers order messages themselves today. Ello adds a link in the bio (posts point to it; caption links can't be tapped) where customers chat or call from the browser; the agent answers routine questions from their catalogue and opening hours and takes the order when the owner can't, then hands over. Ello is a new link next to Instagram DMs and WhatsApp, never a plug-in that answers inside them (facts.md bans "WhatsApp integration"). Inference stops at how they take orders, never that they miss or lose them.

| Cell | Deterministic read | What it tells us (inferred situation) | The angle it earns |
|---|---|---|---|
| S1 x dm | `s1_retail` + `dm` | A seller tells buyers to DM for the price or to order, so they answer price and stock questions in their Instagram inbox themselves | "Turn your Instagram bio into a business desk": a link in the bio where buyers ask price and stock and order; the agent answers from the catalogue while the owner is packing or busy, and passes the rest to them |
| S1 x whatsapp | `s1_retail` + `whatsapp` | A seller sends Instagram buyers to WhatsApp to order, so every buyer switches apps to start the conversation | One link in the bio where buyers chat or call straight away, without switching apps; the agent answers stock and price questions when the owner is busy |
| S1 x both | `s1_retail` + `both` | Orders come in through Instagram DMs and WhatsApp, so the owner follows two inboxes | One link for the bio and the WhatsApp status, so orders start in one place; the agent covers the routine questions and hands over |
| S1 x not_stated | `s1_retail` + `not_stated` | A seller posted an orders-open or pre-order notice without saying where to order (rare: only when a retail seller turns up on a food hashtag) | One link in the bio that tells buyers where to order: tap to chat or call; the agent answers when the owner can't |
| S2 x dm | `s2_food` + `dm` | A food business takes orders in Instagram DMs, so menu, price and opening-hours questions arrive while they're cooking | The link in the bio, pointed to from the menu post, where customers ask what's available and order; the agent answers from the menu and opening hours and takes the routine orders; the owner handles what needs a person |
| S2 x whatsapp | `s2_food` + `whatsapp` | A food business sends customers to WhatsApp to order, so orders depend on someone being on that phone | "Take orders without taking every call": one ordering link where customers chat or call from the browser; the agent takes the routine orders in the rush |
| S2 x both | `s2_food` + `both` | Orders come in through DMs and WhatsApp around an order day, so they land in two places at the busiest time | One link in the bio, pointed to from the menu post and the WhatsApp status, so orders arrive in one place; the agent takes the routine ones at peak |
| S2 x not_stated | `s2_food` + `not_stated` | A food business announced orders open or took pre-orders without saying where to order | One ordering link in the bio, pointed to from the menu post; customers call or chat from it, and the agent answers menu and hours questions and takes the order |

**Angle fix (approved by Fredrick 2026-11-17):** S2 x dm, S2 x both and S2 x not_stated originally put the link "on the menu post". Links in Instagram captions can't be tapped, so the link lives in the bio and the post points to it. The spine below says the same.

**Branch coverage:** `{{business_name}}` exists in every cell. `{{item}}` can be empty in any cell; `{{order_day}}` exists only in S2 cells and can be empty. No angle depends on either.

**Context clauses** (one line at most each, never pick the cell): `{{item}}` names what they posted; `{{order_day}}` (S2) names the order day; personal number in bio adds the approved privacy line ("without your personal phone number ever being exposed").

**Niche-native test:** every angle starts from the instruction in their own post ("DM to order", "WhatsApp to order", "orders open"), which only this business said this fortnight.

## 6. Variables contract [ENGINE]
Approved 2026-11-17.

| Variable | Source field | Exists in branches |
|---|---|---|
| `{{account_key}}` | Internal, not in copy. Instagram handle, lowercase, no "@". The `Sellers` table dedupes on it | All |
| `instagram_handle`, `maps_place_id` (HubSpot) | Internal. Company properties Workflow C writes on every business it contacts (handle lowercase, no "@"; place ID as Maps returns it; empty when unknown). Workflow B stopped writing them 2026-11-25. Suppression looks up the handle, then the place ID, then `domain` | All |
| `domain` (HubSpot) | Internal. Shared with Workflow B. Host from `maps_website`, else `bio_link`; lowercase, no `www` or path; empty for shared hosts (e.g. linktr.ee, wa.me, instagram.com, fresha.com). Both workflows write it when known; never overwrite with a blank. Added 2026-11-25 | When known |
| `ello_last_outreach_date` (HubSpot) | Internal. Shared HubSpot company date property that **both** workflows write on every contact: the sequencer push date (email branch) or `dm_sent_date` (DM branch). Needed because a logged HubSpot note doesn't update "last contacted". Drives the 90-day rule. Added 2026-11-20 | All |
| `{{instagram_handle}}` | Profile username without "@". For the manual DM queue and HubSpot; copy uses it only as written | All |
| `{{business_name}}` | Instagram display name, cleaned by formula: emojis removed, cut at the first `\|` or `•`, or at a dash **with spaces on both sides** (` - `, ` – `), which drops taglines but keeps names like "Jay-Jay Foods"; "Ltd"/"Limited" dropped. If empty, the handle with underscores and dots turned into spaces | All |
| `{{first_name}}` | Owner first name from the Use AI field `owner_first_name`, only if the bio names the owner; else "{{business_name}} team" | All |
| `{{item}}` | Use AI field `item`: short lowercase noun phrase from the caption, no price, emoji or hype | All; can be empty, so copy needs a line that works without it |
| `{{order_day}}` | Use AI field `order_day`: a day name only ("Friday"), only if the caption states one | `s2_food` cells only; can be empty |
| `{{personal_number_in_bio}}` | Signal 6 boolean. Adds the approved privacy line. The number itself is never stored for copy and never quoted | Context clause only |
| `{{email}}` | First found of (a) public profile email, (b) email Claygent finds on the bio link or Maps website. Sent only when Clay validation returns `valid` | Email branch only |
| `branch` | Internal formula. `email` if `{{email}}` exists and validation returns `valid`; otherwise `dm` (no email, or any other validation status) | All ready rows |
| `segment_override` | Internal. Manual review column (`s1_retail` or `s2_food` only) | Reviewed rows only |
| `cac_override` | Internal. Manual review checkbox | Reviewed rows only |
| `is_seller` | Internal. Use AI field (`yes` / `no` / `unclear`) for accounts without a business or creator flag (Section 2) | Accounts without the flag |
| `{{e1l1}}`, `{{e1l2}}`, `{{e2l1}}`, `{{e3l1}}` | Email lines (Copy section), formulas picked by segment, where orders go, the context fields and the personal-number boolean | Email branch |
| `{{dm_l1}}`, `{{dm_q}}` | DM lines (Copy section), formulas picked by segment + context fields (`dm_l1`) and where orders go (`dm_q`) | DM branch |
| `dm_text` | Internal formula: the full DM, "Hi {{first_name}}, " + `dm_l1` with its first letter lowercased **unless the line starts with "I" (as in "I'm")** + the fixed line + `dm_q`, ready to paste | DM branch |
| `dm_sent`, `dm_sent_date` | Internal manual columns ticked by the person who sends the DM; trigger the HubSpot log (Section 3) | DM branch |

**Deliberately excluded:** follower count (filter only; quoting it reads as surveillance), the phone number itself (sensitive; only the boolean is needed), caption text (copy gets `{{item}}` and `{{order_day}}`, never a quote of their post), area (not reliable on Instagram).

## 7. Cadence & channel [ENGINE]
Approved 2026-11-17: **email where a valid email exists; otherwise one manual Instagram DM.**
- Periodicity: always-on; hashtags pulled weekly, posts ≤ 14 days kept (each post gets two chances to be caught).
- Dedupe: one sequence per business per 90 days across all Ello workflows (HubSpot on Instagram handle, Maps place ID and domain). A business in Workflow B never also gets Workflow C. With no domain, check the name by hand against the businesses Workflow B has contacted before sending an email or DM (2026-11-25).
- **Email branch:** Email 1 day 0 (new thread); Email 2 day 3 (same thread); Email 3 day 8 (same thread). Sequencer waits: 3 days, then 5 days.
- **DM branch (no email, or validation not `valid`):** 1 short Instagram DM, sent by hand from an Ello Instagram account, never automated. Same cell routing and context clauses as the email; written by `/campaign-copy` in DM register (reads like a text, no subject, no signature). Never sent to an account that is in the email branch.
- **Logging (both branches):** every business contacted is written to HubSpot with `instagram_handle`, `maps_place_id`, `domain` (when known) and `ello_last_outreach_date`. One Clay HubSpot "create or update company + log activity" step does both branches: for email, on rows the sequencer push column has run on; for DM, on rows where the sender ticked `dm_sent` and filled `dm_sent_date`. Unlogged contacts break the 90-day rule.
- Open before launch: sequencer (not chosen); the Ello Instagram account that sends DMs (NEEDS INPUT); a daily cap on manual DMs (NEEDS INPUT; keep low volume).
- Launch check: Email 3 puts `{{business_name}}` inside a spintax option. Once the sequencer is chosen, convert the spintax to its syntax and send a test to confirm the nested variable fills.

---

## Copy (email + manual Instagram DM)
Written with `/campaign-copy` from Sections 5-6 and `context/messaging/facts.md` only. Sender: Fredrick, Ello. Angle read-back confirmed by Fredrick 2026-11-17.

### Email 1 (day 0, new thread) · APPROVED 2026-11-17

**Subject:** `{your instagram orders|orders on instagram|your latest post}`

```
{{first_name}},

{{e1l1}}

{{e1l2}}

A handle from Ello is one link for your bio: customers tap it to {chat or call you|call or chat with you} from their browser, {no app needed|with no app to download}.

When you're {busy|tied up}, our AI agent answers routine questions from your catalogue and opening hours, and takes the order.

Want me to set up a handle so you can see it?

Fredrick
Ello
```

**`{{e1l1}}`: by segment and which context fields exist (exhaustive, no fallback)**
| Condition | Line |
|---|---|
| `s1_retail` + `item` | You're selling {{item}} on Instagram, so my guess is the price and stock questions are coming in. |
| `s1_retail`, no `item` | You're taking orders on Instagram, so I'd guess buyers are asking about price and stock. |
| `s2_food` + `item` + `order_day` | You're taking {{order_day}} orders for {{item}}, so the menu questions are probably coming in already. |
| `s2_food` + `item`, no `order_day` | You're taking orders for {{item}}, so I imagine the menu questions are coming in. |
| `s2_food` + `order_day`, no `item` | You're taking {{order_day}} orders, so my guess is customers are already asking what's on the menu. |
| `s2_food`, neither | You're taking food orders on Instagram, so I'd guess customers are asking what's on the menu. |

**`{{e1l2}}`: by cell (exhaustive, no fallback)**
| Cell | Line |
|---|---|
| S1 x dm | The post asks buyers to DM you, so those questions land in your Instagram inbox for you to answer. |
| S1 x whatsapp | The post sends buyers to WhatsApp to order, so each one switches apps before they can ask anything. |
| S1 x both | Buyers can order by DM or on WhatsApp, so the messages arrive in two different places. |
| S1 x not_stated | Buyers who like what they see will want a quick way to ask before they order. |
| S2 x dm | Orders come in by DM, so those questions probably reach your inbox while you're cooking. |
| S2 x whatsapp | Orders go through WhatsApp, so they depend on someone being on that phone. |
| S2 x both | Orders come in by DM and on WhatsApp, so they land in two places at the busiest time. |
| S2 x not_stated | Hungry customers will want one quick place to ask what's left and order. |

Facts used: "No app download for customers", bio as a link placement, "Your agent uses your catalogue and opening hours", "takes orders" (value prop Variant B), approved CTA. `{{order_day}}` reads best as a day name ("Friday"). ~75 to 85 words.

### Email 2 (day 3, same thread) · APPROVED 2026-11-17
New value vs E1: the owner stays in control. Subject: `Re:` E1 subject, unchanged.

```
{{first_name}},

{{e2l1}}

{The agent|Our agent} only answers when you turn it on, so you can {keep answering customers yourself|still answer customers yourself} whenever you {like|want}.

{Anything|Whatever comes in that's} sensitive or complex, it passes {straight back|back} to you.

{Would that work with how you take orders?|Does that fit the way you sell?}

Fredrick
Ello
```

**`{{e2l1}}`: by segment (exhaustive, no fallback)**
| Segment | Line |
|---|---|
| `s1_retail` | Chances are you like talking to buyers yourself, since nobody knows your stock better. |
| `s2_food` | Nobody knows your menu better than you, so you may not want an agent talking to your customers. |

Facts used: "Turn the agent on when you need help, or answer customers yourself.", "sensitive or complex questions passed to a person". Answers the owner objection "I'd rather answer customers myself." (personas.md). ~50 words.

### Email 3 (day 8, same thread) · APPROVED 2026-11-17
New value vs E1/E2: where the handle can go, quick setup, number privacy when the personal-number signal is true; polite close. Subject: `Re:` E1 subject, unchanged.

```
{{first_name}},

{{e3l1}}

{It's quick to start:|Starting is quick:} most businesses are set up in one sitting.

{Should I set one up for {{business_name}} to try, or leave it here?|Want one set up for {{business_name}} to try, or shall I leave it here?}

Fredrick
Ello
```

**`{{e3l1}}`: by where orders go x personal number in bio (exhaustive, no fallback)**
| Where orders go | Number in bio | Line |
|---|---|---|
| `dm` | no | Your handle lives in your Instagram bio, so buyers who'd normally DM can tap it too. |
| `dm` | yes | Your handle lives in your Instagram bio, and customers reach you without your personal phone number ever being exposed. |
| `whatsapp` | no | The handle sits in your Instagram bio, so customers can chat or call from there without saving a number first. |
| `whatsapp` | yes | The handle sits in your Instagram bio, and customers reach you without your personal phone number ever being exposed. |
| `both` | no | Your handle can sit in your Instagram bio and on a QR code or business card, so every order starts in one place. |
| `both` | yes | Your handle can sit in your Instagram bio and on a QR code, and customers reach you without your personal phone number ever being exposed. |
| `not_stated` | no | The handle can go in your Instagram bio, on your Google profile and in your ads, so there's one obvious place to order. |
| `not_stated` | yes | The handle can go in your Instagram bio and your ads, and customers reach you without your personal phone number ever being exposed. |

The privacy clause is folded into `e3l1` (not a separate line) so no blank line appears when the signal is false. Facts used: "Most businesses are set up in one sitting.", "without your personal phone number ever being exposed", link placements (Instagram bio, Google profile, ads, QR code, business card). ~40 words.

### Instagram DM (manual, accounts with no valid email) · APPROVED 2026-11-17
One DM, sent by hand from the Ello Instagram account; never to an account in the email branch. No spintax (pasted by hand): the full message is assembled per account in one Clay column, with the first letter of `dm_l1` lowercased after "Hi {{first_name}}, ", unless the line starts with "I" ("I'm messaging…", "I'm writing…" keep the capital).

```
Hi {{first_name}}, {{dm_l1}}

Ello gives you one link for your bio where customers chat or call you from their browser, and our AI agent answers when you're busy.

{{dm_q}}
```

**`{{dm_l1}}`: by segment and which context fields exist (exhaustive, no fallback)**
| Condition | Line |
|---|---|
| `s1_retail` + `item` | Your post about the {{item}} is why I'm messaging. |
| `s1_retail`, no `item` | Your post about taking orders is why I'm writing. |
| `s2_food` + `item` + `order_day` | Your post about {{order_day}} orders for {{item}} got me writing. |
| `s2_food` + `item`, no `order_day` | I'm messaging because of your post taking orders for {{item}}. |
| `s2_food` + `order_day`, no `item` | Your post about {{order_day}} orders is why I'm getting in touch. |
| `s2_food`, neither | I'm writing because of your post about taking food orders. |

**`{{dm_q}}`: by where orders go (exhaustive, no fallback)**
| Where orders go | Question |
|---|---|
| `dm` | Want me to set one up, so buyers who DM you have a quicker way to ask and order? |
| `whatsapp` | Should I set one up, so buyers can reach you without switching to WhatsApp first? |
| `both` | Shall I set one up, so orders start in one place instead of two? |
| `not_stated` | Can I set one up, so customers have one clear place to order? |

Facts used: link in the bio, chat or call from the browser, "AI when you're busy". ~45 words.

### Samples: fully assembled (as a prospect receives them)
Illustrative leads, first spintax option shown.

**Email branch: S2 x whatsapp**, owner "Tunde", business "Acme Kitchen", item "small chops trays", order day "Friday", no personal number in bio.

**Email 1** · Subject: `your instagram orders`
> Tunde,
>
> You're taking Friday orders for small chops trays, so the menu questions are probably coming in already.
>
> Orders go through WhatsApp, so they depend on someone being on that phone.
>
> A handle from Ello is one link for your bio: customers tap it to chat or call you from their browser, no app needed.
>
> When you're busy, our AI agent answers routine questions from your catalogue and opening hours, and takes the order.
>
> Want me to set up a handle so you can see it?
>
> Fredrick
> Ello

**Email 2** (day 3) · Subject: `Re: your instagram orders`
> Tunde,
>
> Nobody knows your menu better than you, so you may not want an agent talking to your customers.
>
> The agent only answers when you turn it on, so you can keep answering customers yourself whenever you like.
>
> Anything sensitive or complex, it passes straight back to you.
>
> Would that work with how you take orders?
>
> Fredrick
> Ello

**Email 3** (day 8) · Subject: `Re: your instagram orders`
> Tunde,
>
> The handle sits in your Instagram bio, so customers can chat or call from there without saving a number first.
>
> It's quick to start: most businesses are set up in one sitting.
>
> Should I set one up for Acme Kitchen to try, or leave it here?
>
> Fredrick
> Ello

**DM branch: S1 x dm**, owner "Ada", item "ankara dresses", no valid email.
> Hi Ada, your post about the ankara dresses is why I'm messaging.
>
> Ello gives you one link for your bio where customers chat or call you from their browser, and our AI agent answers when you're busy.
>
> Want me to set one up, so buyers who DM you have a quicker way to ask and order?
