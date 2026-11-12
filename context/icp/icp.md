# Ello ICP

Status: DRAFT. Built only from `context/docs/research/` (checked 2026-11-12). No customer data exists yet, so this is a hypothesis to test, not a proven profile.

Source codes: [co] company.md, [cu] customers.md, [cm] competitors.md, [mk] market.md, [sg] signals.md, [vo] voice.md. INFERRED = an inference already marked as such in research.

## One line
A registered Nigerian small business (1 to 5 people answering customers), led by retail and social commerce sellers, that sells through Instagram, TikTok, WhatsApp or a storefront and loses sales when nobody answers in time.

## Layer 1: Firmographic
| Attribute | Fit | Evidence |
|---|---|---|
| Country | Nigeria | "initial focus on small and growing businesses in Nigeria"; naira-only pricing [co] |
| City | Lagos first (hypothesis) | Demo chat uses "Yaba"; customer service hiring evidence is Lagos [cu][sg]. Other cities: open question |
| Size | 1 to 5 people handling customers | Plan seats: 1, 2, 5 team members [co]; owner-operator is INFERRED [cu] |
| Verticals | Lead: retail and social commerce; then food and restaurants. Test: salons, real estate, home services. Hold: clinics. See evidence check below | [cu][cm][mk][sg] |
| Legal status | CAC-registered | "We verify every business before it can go live"; badge needs a "registration certificate" [co][cu] |
| Sales model | Enquiry before purchase: price, stock, availability, booking | "Your customers want answers before they buy." [vo]; illustrative chats [cu] |

### Vertical evidence check
Ello lists six use cases, but that list is Ello's marketing, not customer proof (all chats are labelled "Illustrative conversations" [cu]). Ranked here by evidence outside Ello's own copy. Ello has no named customers in any vertical [cu].

| Vertical | Evidence beyond Ello's copy | Tier |
|---|---|---|
| Retail and social commerce | Over half of 250 surveyed Nigerian e-commerce MSMEs sell only via Facebook, Instagram, WhatsApp (GSMA 2023) [mk]; Zana's named testimonials are a fashion boutique, a skincare brand and an electronics reseller (vendor self-reported) [cm]; clothing brands hiring reps in Lagos [sg] | H1: lead |
| Food and restaurants | Lagos takeout restaurant and bakery hiring reps for order handling [cu][sg] | H2: second |
| Salons and beauty | Ello copy only [cu] | Test |
| Real estate | Ello copy only [cu]; signal 9 is INFERRED [sg] | Test |
| Home and professional services | Ello copy only [cu] | Test |
| Clinics and appointments | Ello copy only [cu]; conflicts with the agent passing "sensitive or complex questions" to a person [co] | Hold |

Size caveat: "1 to 5 people" comes from plan seat limits [co], not from customer data. Treat it as Ello's design target, not an observed profile.

## Layer 2: Technographic
| Attribute | Fit | Evidence |
|---|---|---|
| Sales channels | Instagram, TikTok, WhatsApp, Google profile, storefront | Ello placement list [co][vo]; over half of surveyed Nigerian e-commerce MSMEs sell only via Facebook, Instagram, WhatsApp (GSMA 2023) [mk] |
| Contact point today | Owner's personal phone number and WhatsApp | "Not another phone."; privacy pitch [vo]; INFERRED status quo [cm] |
| Website | None, or link-in-bio only | INFERRED [sg] signal 10 |
| Booking / records tools | Google Calendar, Google Sheets, Cal.com (nice to have) | Integrations via Composio [co] |
| Payments | Paystack-compatible | Paystack named as processor [co]. Payments live status: open question |
| Accounts | Google account | "You sign in with Google." [co] |
| WhatsApp API bot users | Possible switchers: per-reply charges from 1 Oct 2026 | [mk] section 1; [sg] signal 5 (INFERRED) |

## Layer 3: Intent signals
Ranked by evidence strength. All detection methods are INFERRED in [sg].

| Signal | Why it points to Ello | Evidence |
|---|---|---|
| Hiring a customer service or order-taking rep | Paying NGN 70,000 to 150,000/month for the job Ello's agent does | Jobberman Lagos listings incl. takeout restaurant and bakery [cu][sg] |
| Personal phone number in Instagram/TikTok bio or posts | Ello's privacy and "one handle" pitch | [sg] signal 2, [vo] |
| Running click-to-WhatsApp or Instagram ads | Paid traffic landing in an inbox that may not be answered | [sg] signal 3 |
| Public complaints about no reply or unreachable | Matches Ello's core pain line | [sg] signal 4 |
| Using a WhatsApp API bot or Meta Business Agent | Cost change: WhatsApp reply charges (Oct 2026), Meta One paid plans (Sept 2026) | [mk] sections 1 and 2 |
| New registration or new storefront | Setting up contact points now | [sg] signal 7 |
| Appointment business with many Google reviews and no booking link | Bookings handled by phone | [sg] signal 8 |
| Property listings with a phone number | "Never lose a property lead to a missed call." | [sg] signal 9 |

## Layer 4: Persona (summary; detail in personas.md)
| Role | Who | Evidence |
|---|---|---|
| Economic buyer | Owner / founder | Pricing plan labels and copy address the owner ("your business") [co][vo] |
| Champion | Person who answers customers: customer service or order-taking rep | Lagos Jobberman listings [cu][sg] |
| Influencer | Person who runs the Instagram/TikTok page and ads | HYPOTHESIS: channel evidence only, no role evidence [sg] |

## Layer 5: Engagement readiness
| Ready | Not ready yet | Evidence |
|---|---|---|
| Has a catalogue, prices and opening hours written down | No product or price list | Agent "uses your catalogue and opening hours" [co] |
| Has registration certificate to hand | Unregistered or struck off | Verification [co]; CAC strike-off of about 100,000 companies (July 2026) [mk] |
| Has a place to put the link (bio, Google profile, card, storefront) | No customer-facing channel | Placement list [co] |
| Customers have smartphones and data | Customers mostly call from basic phones | Calls run "over the internet ... without using the public telephone network" [co] |
| Owner can finish setup "in one sitting" with a Google account | No Google account, no time | [co][vo] |

## Not a fit
| Who | Why | Evidence |
|---|---|---|
| Unregistered or struck-off businesses | Cannot be verified, so cannot go live | [co][mk] |
| Businesses outside Nigeria | Naira-only pricing; Nigeria-only focus stated | [co] |
| Businesses whose customers call from feature phones or have no data | Ello calls need a browser or app over the internet | [co]; VoiceAgents works "with or without internet" [cm] |
| Teams of more than 5 handling customers, or contact centres | Top plan has 5 seats; no enterprise plan | [co]; CallAI enterprise tiers [cm] |
| Businesses needing certified compliance (SOC 2, ISO 27001) | Ello claims none; CallAI does | [cm] |
| Businesses whose enquiries are mostly sensitive or complex (likely includes many clinics) | Agent passes "sensitive or complex questions" to a person, so it handles less of the volume | [co] |
| Customers under 16 / youth-focused businesses | "Ello is not for children under 16." | [co] |

## Open questions (research does not answer)
1. Which verticals do Ello's first live businesses come from? This decides whether H1 (retail and social commerce) holds. Top question for the Ello team.
2. Does the agent speak Pidgin, Yoruba, Hausa or Igbo? Competitors do [cm].
3. Are payments live? Terms and pricing conflict [co].
4. Lagos only, or other Nigerian cities?
5. Which intent signal actually predicts sign-up? No customer data [sg].
6. Is there an influencer role in a 1 to 5 person business?
