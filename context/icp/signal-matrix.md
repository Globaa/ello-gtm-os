# Ello Signal Matrix

Status: DRAFT, work in progress. Validate in a strategy session with the Ello team before building. Built 2026-11-12 with `/signal-matrix` from `context/icp/icp.md`, `context/icp/personas.md` and `context/docs/research/`. Method: `knowledge-base/playbooks/signal-matrix-framework.md`, `knowledge-base/playbooks/workflow-anatomy.md`.

Decisions confirmed by Fredrick on 2026-11-12: company summary as written; rows are vertical segments; full signal mapping for S1, S2, S3.

Source codes: [co] company.md, [cu] customers.md, [cm] competitors.md, [mk] market.md, [sg] signals.md, [vo] voice.md, [icp] icp.md, [pe] personas.md. INFERRED = not supported by research; a hypothesis to test.

## Market data reality (read first)
- Ello's buyers are Nigerian micro and small businesses [co][icp]. The standard B2B stack (Apollo, LinkedIn company data, Crunchbase, BuiltWith) will return little: most have no website, no LinkedIn page and no funding (INFERRED, consistent with the framework's Colombia note).
- Where they are visible: Instagram, TikTok, Google Maps, Meta Ad Library, Jobberman, property portals [sg][icp]. These are the data sources below.
- Ello has zero customers to build lookalikes from [cu]. Every signal here is untested.
- Reach channel is an open question: email addresses for micro businesses are scarce (INFERRED). Instagram DM, WhatsApp and phone may matter more than email. Decide in the GTM system step.

---

## Matrix 1: Segments x Personas

Ello is single-product, so the angle changes per cell, not the product. Columns are the three personas from [pe]. Angles reuse Ello's own words where possible [vo]; they guess the buyer's situation, not their problems.

### Segment definitions

| # | Segment | Target definition (observable) | Priority | Why |
|---|---|---|---|---|
| S1 | Retail and social commerce | Lagos sellers of fashion, beauty products, electronics or accessories whose main storefront is an Instagram or TikTok account; posts mention ordering by DM or WhatsApp; 1 to 5 people [icp] | High (H1) | Most third-party evidence: GSMA, Zana's boutique/skincare/electronics testimonials, clothing brands hiring reps [icp] |
| S2 | Food and takeout | Lagos restaurants, bakeries, small chops and online kitchens taking orders by phone, Instagram or WhatsApp; delivery or pickup [icp] | High (H2) | Takeout restaurant and bakery hiring reps for order handling [cu][sg] |
| S3 | Salons and beauty services | Lagos hair, nail, lash and spa businesses with a Google Maps listing and appointment-based service [icp] | Medium (test) | Ello copy only [icp]; selected for testing because Google Maps makes them easy to find |
| S4 | Real estate agents | Lagos agents and small agencies with active listings on Nigeria Property Centre | Low (test, not mapped) | Ello copy only [icp] |
| S5 | Home and professional services | Plumbers, electricians, cleaners and similar taking jobs by phone | Low (test, not mapped) | Ello copy only [icp]; no company examples found |
| Hold | Clinics | Not targeted | Hold | Agent passes "sensitive or complex questions" to a person [co][icp] |

### Angle grid

| Segment | Owner (economic buyer) | Customer service / order-taking rep (champion) | Social / ads runner (influencer, HYPOTHESIS) |
|---|---|---|---|
| S1 Retail | "They were ready to buy. You were serving someone else." Ello answers price and stock questions and takes the order while you pack the last one [vo][cu] | One link for calls and chats, so you stop switching between DMs, WhatsApp and phone; the agent handles "is size 12 in stock?" and hands you the rest (INFERRED from [co]) | "Turn your Instagram bio into a business desk." One link for the bio and ads; customers tap and talk, no app [cu][vo] |
| S2 Food | "Take orders without taking every call." The agent answers menu, price and opening-hours questions and captures the order [cu][co] | Fewer repeat "are you open?" questions in the lunch rush; the agent collects the order details for you (INFERRED from [cu] outcome line) | Put one ordering link on the menu post and the ad (INFERRED) |
| S3 Salons | "Turn "Are you available?" into a booking." The agent books slots while your hands are busy [cu] | Bookings land in the calendar without phone tag; connects to Google Calendar or Cal.com [co] | One booking link for the bio and Google profile [co] |
| S4 Real estate | "Never lose a property lead to a missed call." [cu] | Open question: role not evidenced | Open question |
| S5 Home services | "Turn inbound enquiries into organised jobs." [cu] | Open question | Open question |

### Company examples
Found by web search on 2026-11-12. Real businesses, but fit (size, contact setup, registration) is NOT verified. Treat them as test records, not a target list.

| Segment | Examples | Source |
|---|---|---|
| S1 Retail | Web search did not return verifiable small Lagos Instagram sellers. Requires list building with: Instagram hashtag search (#lagosvendor, #lagosboutique, #shopinlagos, #lagosthriftstore) + bio contains a Nigerian mobile number or wa.me link + under 50k followers (INFERRED filters) | n/a |
| S2 Food | The Small Chops Company, OfadaBoy ("use Instagram and WhatsApp to take daily orders"); Gourmet Twist (Lekki online bakery); Fluff Kitchen (online kitchen); Cheri Crumbs Confections; Boyes Homemade Pastries; Bigchops | https://www.discoverlagos.ng/post/how-social-media-is-transforming-lagos-businesses ; https://connectciti.com/listing/gourmet-twist-best-banana-bread-and-meals-in-lagos ; https://foodieinlagos.com/review-fluff-online-kitchen/ ; https://connectciti.com/listing/cheri-crumbs-confections ; https://connectciti.com/listing/boyes-homemade-pastries ; https://connectciti.com/listing/bigchops-2 |
| S3 Salons | Eden Salon (Lekki); Blueberry Locs (Lekki Phase I); Fluff n Puff (Lekki Phase I); Anoint Hair Restoration Center (Lekki); Hello Texture (Lekki); Lipa Hair (Victoria Island); TasalaHQ Hair and Beauty (Lekki, Ikoyi; 1,020 reviews, may be above 5 staff) | https://www.fresha.com/lp/en/bt/hair-salons/in/ng-lagos |
| S4 Real estate | Azora Homes (Ajah); SFM Homes and Properties (Ajah); Uzizo Realty (Lekki); Outstanding Real Estate Limited (Lekki); Perpetual Homes and Real Estate (Lekki); New Era Properties Pro (Ajah); The Luxe Girl Agency (Ajah); Able God (Ajah) | https://nigeriapropertycentre.com/agents/sfm-homes-and-properties-19894?cid=2 and sibling agent pages |
| S5 Home services | None found. Requires Google Maps search: "plumber" / "electrician" / "cleaning service" + Lagos | n/a |

New finding: the S3 examples are all listed on Fresha, which offers online booking, prepayment and reminders (Fresha pages above). Fresha is an S3 competitor not covered in competitors.md. Salons already on Fresha are a weaker fit for the booking angle (INFERRED).

---

## Phase 2: Segment assessment

| Segment | Signal density | Product fit | Penetrability (data access) | Pursue? |
|---|---|---|---|---|
| S1 Retail | High: Instagram, TikTok, Meta ads, jobs (INFERRED) | High: matches Ello's "Instagram bio" pitch [cu]; strongest evidence [icp] | Medium: Instagram data is cheap but there is no business registry match; CAC status unknown per seller | Yes (confirmed) |
| S2 Food | High: Instagram, Google Maps, reviews, jobs (INFERRED) | High: order-taking is a core Ello feature [co] | High: Google Maps listings plus Instagram | Yes (confirmed) |
| S3 Salons | Medium: Google Maps, reviews (INFERRED) | Medium: booking fits, but Fresha competes (see above) | High: Google Maps | Yes, as test (confirmed) |
| S4 Real estate | Medium: portal listings | Medium: Ello copy only | High: Nigeria Property Centre agent pages | Not now |
| S5 Home services | Low | Medium | Medium | Not now |

---

## Matrix 2: Signals

Every signal starts with the signal, then applies the ICP filter [workflow-anatomy]. Tool prices are per call from treg `catalog_search` on 2026-11-12 (no paid calls made). All volume estimates are INFERRED until a test run.

**Scores** on the four tests from workflow-anatomy.md: Reason = clear buying reason, Vol = enough volume, Data = data you can get, Story = natural story. H / M / L.

### Universal signals (one build, keyword swap per segment)

| ID | Signal (observable event) | Mother intent | Data source | Detection tool | Reason | Vol | Data | Story | Status |
|---|---|---|---|---|---|---|---|---|---|
| U1 | Posted a job in the last 30 days for customer service rep, order-taker, sales rep, receptionist or front desk, Lagos | Hiring specific roles | Jobberman category pages (e.g. https://www.jobberman.com/jobs/customer-service-support/lagos); LinkedIn Jobs | Jobberman: Clay HTTP or `tools/crawl4ai` scrape (no treg endpoint found). LinkedIn: treg `apify.linkedin.search.jobs` ($0.001/call) | H: they are budgeting NGN 70,000 to 150,000/month for the job the agent does [cu]; Ello Starter is ₦15,000 [co] | M: Lagos customer service category runs to multiple pages (INFERRED) | M: Jobberman has no API; scrape needed; employer names are often small brands | H: "Saw you're hiring someone to handle orders" | VERIFIED (PARTIAL): scrape to be tested |
| U2 | Instagram or TikTok bio contains a Nigerian mobile number (+234, 070, 080, 081, 090, 091) or a wa.me link | Website content mentions | Instagram and TikTok profiles | treg `tikhub.instagram.user.profile` ($0.001), `crawl4ai.tiktok.user.profile` ($0.00064) + regex | M: matches Ello's privacy and "not another phone" pitch [vo] | H (INFERRED) | H: cheap, reliable (99.9% success on tikhub) | M | VERIFIED (COMPOUND): static attribute, not timing. Use only as a filter on top of N1, N2 or U3 |
| U3 | Started a Meta ad (Instagram/Facebook, incl. click-to-WhatsApp) in the last 30 days, Nigeria | Niche play: paid acquisition | Meta Ad Library | treg `crawl4ai.meta-ads.library.search` ($0.00165), `apify.meta-ads.library.search` ($0.005) | H: paying for traffic now; WhatsApp promotes "ads that click to WhatsApp" to Nigerian SMEs [sg] | M (INFERRED) | M: Nigeria coverage of non-political ads in the library must be tested | H: "Saw your new ad for X" (situation, not a gap) | VERIFIED (PARTIAL) |
| U4 | Google review in the last 90 days mentions not being reachable: "no response", "didn't pick", "couldn't reach", "no reply", "never called back" | Niche play: service complaint | Google Maps reviews | treg `treg.google.maps.reviews` ($0.00015) on places from `crawl4ai.google.serp.maps` ($0.00023) | H: Ello's core pain, "A missed call." [vo] | L to M (INFERRED): needs a storefront with reviews | H: cheap, 99% success | M: must not quote the complaint back (guardrail: don't diagnose gaps). Use the situation, not the review | VERIFIED (PARTIAL): S2, S3 only |
| U5 | New business or branch: Google Maps listing whose first review is under 90 days old, or Instagram post with #nowopen / #grandopening + Lagos | Recently incorporated / new location | Google Maps; Instagram hashtags | treg Google Maps search + reviews (above); `anyapi.instagram.hashtag.posts` ($0.002) | M: setting up contact points now [sg] | M (INFERRED) | M: first-review date is a proxy for opening date | H: "Congrats on opening in Lekki" | VERIFIED (PARTIAL) |
| K1 | Fundraising | Funding | Crunchbase | n/a | | | | | KILLED: micro businesses are not VC-funded (framework rule for non-VC markets) |
| K2 | New leadership in 90 days | New leadership | Apollo / LinkedIn | n/a | | | | | KILLED: owner-run businesses; no leadership changes to track |
| K3 | Headcount growth | Hypergrowth | Apollo / LinkedIn | n/a | | | | | KILLED: these businesses have no reliable LinkedIn headcount (INFERRED) |
| K4 | Using a WhatsApp API bot or Meta Business Agent (cost change from 1 Oct 2026, Meta One) | Competitor customers / regulatory change | No reliable source: API use and Meta AI replies are not visible from outside; BuiltWith needs a website | n/a | H [mk] | ? | L | H | KILLED as a mother intent. Keep as a story line when another signal fires and a competitor widget (Zana, Obeks) is seen |
| K5 | Seasonal peaks (Black Friday, December, Valentine's) | Niche play | Calendar | n/a | | | | | KILLED as a signal (same for everyone, no timing per account). Use as a send-timing overlay. Not researched [sg] |

### Niche-specific signals

| ID | Segment | Signal (observable event) | Mother intent | Data source | Detection tool | Reason | Vol | Data | Story | Status |
|---|---|---|---|---|---|---|---|---|---|---|
| N1 | S1 | Posted in the last 14 days with "DM to order", "send a DM", "WhatsApp to order", "price in DM" in the caption, on Lagos vendor hashtags (#lagosvendor, #lagosboutique, #shopinlagos) | Website content mentions | Instagram hashtag posts + profile | treg `anyapi.instagram.hashtag.posts` ($0.002, 99.4% success; routed treg endpoint only 68%) then profile ($0.001) | H: orders are taken in DMs today; Ello: "Turn your Instagram bio into a business desk." [cu] | H (INFERRED) | H | H: reference the product they just posted | VERIFIED |
| N2 | S2 | Food business posting a daily or weekly menu / "orders open" / "pre-order" in the last 14 days (#smallchopsinlagos, #lagosbaker, #lagosfoodvendor) | Website content mentions | Instagram hashtag posts | Same as N1 | H: The Small Chops Company and OfadaBoy "use Instagram and WhatsApp to take daily orders" (Discover Lagos, above) | M (INFERRED) | H | H: "Saw orders are open for Friday" | VERIFIED |
| N3 | S3 | Salon on Google Maps with 50+ reviews and no booking link (no Fresha, Booksy or Cal.com URL in the listing website field) | Niche play: physical-world | Google Maps search "hair salon" / "nail salon" + Lagos areas | treg `crawl4ai.google.serp.maps` ($0.00023) | M: bookings likely by phone (INFERRED [sg] signal 8) | M (INFERRED) | H | M: static attribute; pair with U4 or U1 for timing | VERIFIED (COMPOUND) |
| N4 | S1, S2, S3 | CAC registration check, and not on the CAC strike-off list | ICP gate (not a signal) | CAC public search (search.cac.gov.ng, per-name lookup); CAC strike-off lists [mk] | Clay HTTP per name (INFERRED: no bulk API known) | n/a: required to go live [co] | n/a | M: name matching from Instagram handles is fuzzy | n/a | GATE: run after a signal fires, before outreach |
| H1 | S4 (not mapped) | Agent with 20+ active listings on Nigeria Property Centre that show a mobile number | Niche play | nigeriapropertycentre.com agent pages | Clay HTTP / `tools/crawl4ai` | M | M | H | M | ON HOLD with S4 |

### Compounding check
- 5 universal signals live (U1 to U5) out of 10 considered (5 killed). U1, U3 and U5 run the same build for S1, S2 and S3 with keyword swaps; U4 works only where there is a storefront (S2, S3).
- 3 niche signals live (N1, N2, N3) plus 1 ICP gate (N4). N1 and N2 are the same build (Instagram hashtag + caption keywords) with different hashtags, so in practice one build.
- Net: about 4 distinct builds (job scrape, Instagram hashtag/profile, Meta Ad Library, Google Maps + reviews) cover all three priority segments.

### Matrix 2 by segment

| Signal | S1 Retail | S2 Food | S3 Salons |
|---|---|---|---|
| U1 Hiring rep | "customer service", "sales rep", "online sales" | "order taker", "customer service", "cashier" | "receptionist", "front desk" |
| U2 Personal number in bio | Filter on N1, U3 | Filter on N2, U3 | Filter on N3 |
| U3 New Meta ad | Fashion, beauty, gadgets keywords | Food, delivery keywords | Salon, braids, nails keywords |
| U4 Unreachable in reviews | Only sellers with a storefront | Yes | Yes |
| U5 New opening | #nowopen + boutique | New food place on Maps | New salon on Maps |
| N1 DM-to-order posts | Yes | n/a | n/a |
| N2 Orders-open posts | n/a | Yes | n/a |
| N3 Many reviews, no booking link | n/a | n/a | Yes |
| N4 CAC gate | Yes | Yes | Yes |

---

## Top signals to build first

| Rank | Signal | Segments | Why first |
|---|---|---|---|
| 1 | N1 DM-to-order Instagram sellers (+ U2 personal-number filter) | S1 | Hits the lead segment (H1) at the highest volume, with the cheapest and most reliable data ($0.002 + $0.001 per record). The story is the seller's own post. Same build serves N2 |
| 2 | U1 Hiring a customer service or order-taking rep | S1, S2, S3 | Clearest buying reason in the research: a NGN 70,000 to 150,000/month hire vs ₦15,000/month Starter [cu][co]. Real timing. One build across all three segments |
| 3 | N2 Food "orders open" posts | S2 | Near-free once N1 exists (hashtag swap). Strongest direct evidence: named Lagos food brands take daily orders on Instagram and WhatsApp |
| 4 | U3 New Meta ad in Nigeria | S1, S2 | Paid traffic now means enquiries now; natural, non-diagnosing story. Needs a coverage test of the Ad Library for Nigeria first |
| 5 | U4 Unreachable in Google reviews (+ N3 for salons) | S2, S3 | Most direct evidence of Ello's exact pain, cheapest data ($0.00015 a call). Lower volume, so it is a high-relevance add-on, not a volume engine |

Before any of these runs: test N4 (CAC gate) on 20 records, since Ello will not let unregistered businesses go live [co]. If most Instagram sellers fail the gate, S1 shrinks and S2/S3 move up.

---

## Matrix 3: Workflow tracker

| W # | Status | Workflow name | Mother intent | Segment | Persona | Channel | Clay URL | Copy URL | Total leads |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Planned | Instagram DM-to-order sellers | Website content mentions (N1) | S1 | Owner | TBD | | | |
| 2 | Planned | Hiring customer service / order-taking reps | Hiring specific roles (U1) | S1, S2, S3 | Owner | TBD | | | |
| 3 | Planned | Food orders-open posts | Website content mentions (N2) | S2 | Owner | TBD | | | |
| 4 | Planned | New Meta ads in Nigeria | Paid acquisition (U3) | S1, S2 | Owner, social runner | TBD | | | |
| 5 | Planned | Unreachable in Google reviews | Service complaint (U4) | S2, S3 | Owner | TBD | | | |
| 6 | Planned | New openings | New location (U5) | S1, S2, S3 | Owner | TBD | | | |
| 7 | Planned | Busy salons without online booking | Physical-world (N3) | S3 | Owner | TBD | | | |

Other tracker columns from the framework (campaigns, launch date, enrichments, iterations, bounce) are added when a workflow moves to Building.

## Open questions for the strategy session
1. Which segments are Ello's first live businesses in? This confirms or reorders S1/S2/S3 [icp].
2. Outreach channel for micro businesses: email, Instagram DM, WhatsApp or phone? Shapes every workflow.
3. Does the CAC gate (N4) cut S1 sharply?
4. Is Fresha a real competitor in S3?
5. Do any of the S2/S3 example businesses already use Ello?

## Not done
- No Google Sheets push: `WORKFLOW_TRACKER_SPREADSHEET_ID` is not set in `.env`.
- No paid treg calls; volumes are estimates until a test run.
