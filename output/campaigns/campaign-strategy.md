# Ello Campaign Strategy

**Status:** DRAFT, 2026-11-13. Built with `/campaign-strategy` from `CLAUDE.md`, `context/` (icp, personas, signal-matrix, positioning, messaging, case-studies, research) and `knowledge-base/playbooks/workflow-anatomy.md`.

Source codes: [co] company.md, [cu] customers.md, [cm] competitors.md, [mk] market.md, [sg] signals.md, [vo] voice.md, [sm] signal-matrix.md, [icp] icp.md, [pe] personas.md. INFERRED = reasoning, not a sourced fact.

**Target audience:** Lagos micro and small businesses (1 to 5 people) that take customer questions, orders or bookings by phone, Instagram and WhatsApp; S1 retail and social commerce, S2 food, S3 salons first [icp][sm].
**Core offer:** one handle (ello.africa/@business) customers call or chat from the browser, with an AI agent that answers, takes orders and books when the owner can't [co]. Plans ₦0, ₦15,000 and ₦40,000 a month [co].
**Differentiators:** calls and chat behind one link; no app and no SIM; personal number stays private; verified badge [co][cm].

---

## Customer discovery analysis

| Company | Industry | Size | Key metric | Problem solved |
|---|---|---|---|---|
| Not found | | | | |

- Ello has **no named customers, case studies or testimonials** [cu]. The Android app is "in testing with our first businesses", unnamed [co].
- Patterns can only come from third-party evidence: Lagos food, bakery and clothing brands hiring customer service reps [sg]; over half of 250 surveyed Nigerian e-commerce MSMEs sell only through Facebook, Instagram and WhatsApp (GSMA 2023) [mk]; Zana's testimonials are a boutique, a skincare brand and an electronics reseller [cm].
- **Targeting challenge:** Ello lists six verticals as equal [co]. The evidence supports retail and food first; salons are a test because Fresha already offers them booking [sm]. Keep S4 real estate and S5 home services as later tests.
- **Lookalike seeds:** none usable. Idea 20 is blocked until the Ello team names live businesses and adds them to `context/messaging/facts.md`.

**Volume note:** no list has been pulled yet, so no monthly counts exist. Volume is a band (High / Medium / Low) based on the source and the signal matrix, all INFERRED. Each Tier 1 idea needs one small test pull to get a real count before it is built.

---

## Campaign ideas, ranked

Ranked by **signal strength** (how clearly it means "needs Ello now") and then **data ease** (cheap, reliable, no manual research). Difficulty covers list building, enrichment and finding a contact.

| # | Campaign | Mother intent | Segment · persona | Signal · data source | Angle | Volume / month | Difficulty | Signal · Data | Tier |
|---|---|---|---|---|---|---|---|---|---|
| 1 | DM-to-order sellers | Website content mentions (social posts; flagged adaptation) | S1 · owner | Post in last 14 days says "DM to order", "price in DM", "WhatsApp to order" on Lagos vendor hashtags · Instagram hashtag posts + profile (treg, $0.002 + $0.001) [sm N1] | "Turn your Instagram bio into a business desk." Customers tap one link to call or chat, and the agent answers price and stock questions while they're packing orders [cu] | High (INFERRED) | Low to medium: data cheap and reliable; contact email scarce | H · H | 1: strongest data, lead segment |
| 2 | Food orders-open posts | Website content mentions (social posts) | S2 · owner | Menu, "orders open" or "pre-order" post in last 14 days (#smallchopsinlagos, #lagosbaker) · Instagram hashtag posts [sm N2] | "Take orders without taking every call." One ordering link on the menu post; agent answers menu and opening-hours questions [cu] | Medium (INFERRED) | Low once #1 is built (hashtag swap) | H · H | 1: same build as #1 |
| 3 | Hiring a customer service or order-taking rep | Hiring specific roles | S1, S2, S3 · owner | Lagos job post ≤ 30 days, customer-facing title · Jobberman scrape, LinkedIn Jobs (treg $0.001) [sm U1] | The new hire covers working hours; one link and an agent answer when nobody can, then hand over. Never "replace the hire" | Medium (INFERRED: Lagos customer service category runs to several pages) | Medium: Jobberman needs a scrape; employer to Instagram match | H · M | 1: spec in progress (Workflow B) |
| 4 | New Meta ad running | Niche-specific play: paid acquisition (new intent, flagged) | S1, S2 · owner, social runner | Ad started ≤ 30 days, Nigeria · Meta Ad Library (treg $0.00165) [sm U3] | Paying for clicks means enquiries arrive now; one link on the ad so customers can call or chat, answered even after hours | Medium (INFERRED) | Medium: Nigeria coverage of the Ad Library needs a test first | H · M | 1: test coverage first |
| 5 | Golden ICP stack | Niche-specific play | S1 · owner | #1 AND personal number in bio AND new Meta ad AND CAC registered [sm N1+U2+U3+N4] | What a rep writes after 10 minutes: their product post, their ad, their number in the bio. One link, number kept private, agent answers ad traffic | Low (stacked) | Medium: 3 enrichments + CAC gate | H · M | 2: low volume, highest relevance |
| 6 | Hard to reach in Google reviews | Niche-specific play: service complaint | S2, S3 · owner | Review in last 90 days says "no response", "didn't pick", "couldn't reach" · Google Maps reviews (treg $0.00015) [sm U4] | Situation, not blame: customers try to reach them while they're busy. Agent picks up calls and chats. Never quote the review back | Low (INFERRED) | Low: cheap, 99% success | H · H | 2: low volume, add-on |
| 7 | Personal number in bio | Website content mentions (bio) | S1, S2 · owner | Bio has a Nigerian mobile or wa.me link (filter on #1/#2 lists) · Instagram/TikTok profile + regex [sm U2] | "Customers reach your business without your personal phone number ever being exposed." [co] | High (as a filter) | Low: regex on data already pulled | M · H | 2: filter, no timing |
| 8 | TikTok sellers taking orders | Website content mentions (social posts) | S1 · owner, social runner | TikTok bio with "call/WhatsApp to order" + number, Lagos · TikTok profile (treg $0.00064) [sm U2] | One link for the TikTok bio; viewers tap to call or chat without leaving to search a number | Medium (INFERRED) | Medium: hashtag-to-profile discovery on TikTok untested | M · H | 2: untested source |
| 9 | Closed but still asked | Niche-specific play (new, flagged) | S2, S3 · owner | Google Maps hours show Sunday closed or closing by 6pm · Maps listing (treg $0.00023) | "A question asked after hours." [vo] The agent answers with their own opening hours and takes the order for when they open | Medium (INFERRED) | Low: one Maps field | M · H | 2: creative stretch, static attribute |
| 10 | New openings | Recently incorporated businesses (new location) | S1, S2, S3 · owner | First Google review < 90 days, or #nowopen / #grandopening + Lagos post · Maps reviews, Instagram hashtags [sm U5] | Setting up contact points now: give the business a handle before the phone number spreads | Medium (INFERRED) | Medium: first review is only a proxy for opening date | M · M | 2 |
| 11 | Busy salons, no booking link | Niche-specific play (physical world) | S3 · owner | 50+ Maps reviews, no Fresha/Booksy/Cal.com link · Maps listing [sm N3] | "Turn "Are you available?" into a booking." [cu] Agent books while hands are busy | Medium (INFERRED) | Low | M · H | 2: Fresha competes |
| 12 | Creative ideas | Niche-specific play | S1, S2, S3 · owner | Base list from #1, #2, Maps; AI column reads bio, highlights, menu, recent captions | "I had an idea for {{business_name}}": three questions their customers clearly ask (from their own posts) that the agent could answer | High (base list) | Medium: AI column + review of outputs | L · H | 2: no timing signal |
| 13 | Seasonal rush overlay | Niche-specific play (send-timing overlay) | S1, S2 · owner | Lists #1/#2 sent 2 to 3 weeks before December, Valentine's, Black Friday [sm K5] | Busy season means more enquiries than hands; the agent covers the rush | Same as #1/#2 | Low: timing only | M · H | 2: overlay, not a signal |
| 14 | Social media managers (creative, non-ICP) | Niche-specific play: partner / referral (new, flagged) | Lagos freelancers and small agencies running Instagram pages for vendors · the manager | Bio says "social media manager" / "SMM" + Lagos, or Maps "social media agency Lagos" · Instagram profile, Maps | Their clients' DMs pile up while they post. One link per client page; they set it up as part of their service. Needs a referral offer from Ello (Not found) | Medium (INFERRED) | Medium: offer must exist first | M · M | 2: blocked on partner offer |
| 15 | Real estate agents with many listings | Niche-specific play | S4 · agent-owner | 20+ active listings with a mobile number · Nigeria Property Centre agent pages [sm H1] | "Never lose a property lead to a missed call." [cu] | Medium (INFERRED) | Low: portal pages are structured | M · H | 3: S4 on hold |
| 16 | Home services by phone | Niche-specific play | S5 · owner | Maps listing "plumber", "electrician", "cleaning service" + Lagos with phone only · Maps [sm] | "Turn inbound enquiries into organised jobs." [cu] | Unknown (no examples found [sm]) | Medium | L · M | 3: no evidence yet |
| 17 | WhatsApp API bot users | Competitor customers | S1, S2 · owner | Bio link or website shows a Zana or Obeks link/widget · profile link domain match (untested) | WhatsApp Business Platform charges for some replies from 1 Oct 2026 [mk]; Ello's link doesn't run through it, and adds calls | Low (INFERRED) | High: detection unverified [sm K4] | M · L | 3: detection unproven |
| 18 | New customer service hire (New Hire) | New hires in target dept | S1, S2, S3 · new rep (champion), cc owner | Person started a customer service / front desk role at a small Lagos business < 90 days · LinkedIn people search (treg, price to check) | First weeks in the job: the agent takes the repeat questions and passes them the conversations that need a person, with customer history [co] | Low (INFERRED: micro businesses rarely list staff on LinkedIn) | High: thin LinkedIn coverage | M · L | 3: thin data |
| 19 | Business registration agents (creative, non-ICP) | Niche-specific play: partner / referral (new, flagged) | Lagos CAC registration agents and consultants · the agent | Instagram/Maps "CAC registration" + Lagos · profile, Maps | Their clients just registered and need a contact point; Ello checks the same registration certificate for its verified badge [co] | Low (INFERRED) | Medium: needs referral offer | L · M | 3: speculative |
| 20 | Lookalike of first Ello businesses | Lookalike of customers | Whatever segments live users are in · owner | Ello's first live businesses as seeds, then same hashtags, Maps category and area · Ello team list (not available) | Similar businesses already use Ello (only names approved in facts.md) | Unknown | Blocked: no seeds [cu] | H · L | 3: blocked until customers are named |

---

## Overviews for handoff

Each overview gives the hook, the personalised elements, the CTA style, and how the idea differs from the rest. Every specific claim must come from `context/messaging/facts.md` when copy is written. CTAs are questions; no "book a call", never the word "free".

1. **DM-to-order sellers.** Hook: the product they posted this week. Personalised: product name from the caption, handle. CTA: "Want me to set up a handle so you can see it?" Differs: the lead-segment volume engine.
2. **Food orders-open.** Hook: the orders-open post ("Saw orders are open for Friday"). Personalised: menu item, order day. Differs: food words, same build as #1.
3. **Hiring a rep.** Hook: `{{role_title}}` from the post. Routes on segment × role channel (12 cells, spec Sections 1-4 approved). Differs: highest-intent timing; salary never used.
4. **New Meta ad.** Hook: the ad's product or offer. Personalised: ad start date, product. Angle is about enquiries arriving, never ad performance. Differs: reaches the social/ads runner too.
5. **Golden ICP.** Hook: the product post. Body ties the ad, number in bio and one link together. Small list, written close to hand-crafted. Differs: three stacked signals.
6. **Hard to reach in reviews.** Hook: their storefront and area, not the review. The insult test applies strictly. Differs: closest to Ello's exact pain.
7. **Personal number in bio.** Hook: privacy. Use as an extra line on #1/#2 rather than a standalone send.
8. **TikTok sellers.** Like #1 on TikTok. Hook: their latest video's product. Test discovery first.
9. **Closed but still asked.** Hook: their opening hours ("You close at 6 on weekdays"). Angle: the agent answers with those hours and takes the order for the morning. Differs: uses a plain listing field as timing.
10. **New openings.** Hook: congratulations on the new spot in `{{area}}`. Angle: set up the contact point at the start. Differs: catches businesses before habits form.
11. **Busy salons.** Hook: review volume ("lots of happy clients on Google"). Check for Fresha before sending.
12. **Creative ideas.** Hook: "I had an idea for {{business_name}}." Three bullets: questions their customers ask in comments, each with how the agent answers it. Requires a hard enum check that each idea maps to an Ello feature [co].
13. **Seasonal rush.** Same lists as #1/#2; copy opens on the season. Timing only.
14. **Social media managers.** Peer register: "You run pages for vendors; this gives every client one link." Needs Ello to define a referral or partner offer first.
15. **Real estate.** Hook: a current listing and area. Hold until S4 is reopened.
16. **Home services.** Static, no-AI candidate. Hold until a Maps test shows volume.
17. **WhatsApp API bot users.** Hook: the 1 Oct 2026 change. Only send where detection is certain; never guess their setup.
18. **New customer service hire.** Champion route: written to the new rep, short, "for your first weeks". Owner copied only if the rep replies.
19. **CAC registration agents.** Partner register; offer referral. Needs offer.
20. **Lookalike.** Same build as the source signal, plus "businesses like {{social_proof}}". Only when facts.md holds approved names.

---

## No-AI campaigns

**Home services by phone (#16) or Busy salons (#11) as static sends**
- Why it works without AI: the list filter is the message. Every record shares the same situation (taking work by phone, or busy with bookings), so one well-written static email fits all.
- Core message: their category in one line, then Ello's one link that customers call or chat and the agent that answers when hands are full, then one question CTA. Heavy spintax, under 70 words.

---

## Front-end offer suggestions

1. **"We'll hold your handle."** Offer to set up `@theirbusiness` and send them the link to test: they call their own handle and hear the agent answer with their opening hours. Value in one sitting ("Most businesses are set up in one sitting." [vo]). Needs Ello's OK to create handles on request; never create one without consent.
2. **Live demo call.** Point to the live demo so they can call a business handle themselves. Blocked: ello.africa/ellodemo returned HTTP 500 on 2026-11-12 [co]. Check first.
3. **The ₦0 plan.** "AI chat and voice through your Ello link" on a ₦0 /month plan [co] is a self-serve quick win: the owner adds the link to the bio and tries it on real customers before paying. Write it as "₦0 /month", never "free".

---

## What to do next
- Tier 1 order: #1 and #2 share one build; #3 is already being specced (Workflow B); #4 needs an Ad Library coverage test.
- Before any of them: run the CAC gate test on 20 records [sm], and pull one small test list per Tier 1 source to replace the volume bands with counts.
- Decisions blocking ideas: reach channel for micro businesses (email vs DM/WhatsApp), Ello partner/referral offer (#14, #19), named customers (#20).
