# Workflow B: Lagos businesses hiring a customer service or order-taking rep

**Status:** IN PROGRESS · Sections 1-4 approved 2026-11-13 · Sections 5-7 (routing matrix with angles, variables contract, cadence and channel) pending · NOT STRUCTURE-APPROVED · copy blocked

Built with `/build-workflow-spec` from `context/icp/icp.md`, `context/icp/personas.md`, `context/icp/signal-matrix.md` (signal U1) and `context/docs/gtm-system.md`. `context/messaging/facts.md` does not exist yet: every specific fact stays a `{{placeholder}}`. No copy in this file.

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
| Size | 1 to 3 open customer-facing roles at the employer; Instagram followers under 50k if an account is found (rule to test) | icp.md (5-seat top plan), gtm-system.md step 5 |
| Registration | CAC-registered and not on a strike-off list | icp.md, gtm-system.md step 5 |
| Trigger | Title matches `customer service\|customer care\|customer support\|order\|sales rep\|sales representative\|online sales\|receptionist\|front desk\|client service`, posted ≤ 30 days ago | signal-matrix.md U1 |
| Recipient | The owner (economic buyer); if no owner name is found, the business ("{{business_name}} team") | personas.md, gtm-system.md step 7 |

**Exclude:**
| Who | Why | How it's read |
|---|---|---|
| Recruiters, staffing firms, call centres, outsourcers posting for clients | Not the buyer | Employer name matches `recruit\|staffing\|outsourc\|bpo\|human resource\|manpower\|consult`, OR AI column returns `recruiter` |
| "Confidential" or missing employer | No account to match | Employer empty or contains `confidential` |
| Employers outside S1 to S3 (banks, telcos, fintech, logistics, clinics) | Not ICP; clinics on hold | AI segment column returns `other` or `clinic` |
| More than 3 open customer-facing roles | Too big for Ello's plans | Count of matching posts per employer > 3 |
| Competitors: Viqe Systems (Zana), CherryBrooks (CallAI), Obeks, NativeTalk, PressOne, Salescabal (Bumpa), Fresha | Competitors | Employer name on blocklist |
| Existing Ello users | Already customers | Needs Ello sign-up export (open decision, gtm-system.md) |
| HubSpot customer, open deal, or contacted in last 90 days | Suppression | `account_key` lookup in HubSpot |
| Unregistered or struck off | Ello cannot verify them | CAC lookup (gate) |

## 3. Sources [ENGINE]
| Purpose | Source | Tool | Cost per call (treg, 2026-11-12) |
|---|---|---|---|
| Trigger (main) | Jobberman Lagos customer service category (https://www.jobberman.com/jobs/customer-service-support/lagos) | `tools/crawl4ai` or Clay HTTP; no treg endpoint | Free locally |
| Trigger (secondary) | LinkedIn Jobs, Lagos | treg `apify.linkedin.search.jobs` | $0.001 |
| Employer to account | Google Maps search by employer name + Lagos | treg `crawl4ai.google.serp.maps` | $0.00023 |
| Instagram account | Maps website field, or link on the job page | Parse. Instagram lookup by business name not yet confirmed in treg (OPEN) | $0 |
| Instagram profile and posts | Profile, recent captions | treg `tikhub.instagram.user.profile` | $0.001 |
| Active Meta ad (stack) | Meta Ad Library | treg `crawl4ai.meta-ads.library.search` | $0.00165 |
| Segment and recruiter classification | Job description + employer name + Maps category | Clay AI column (Claude), fixed output list | Model cost |
| Registration gate | CAC public search | Clay HTTP, per-name lookup | Free |
| Owner name, email | Instagram bio, Maps owner replies, CAC record; Clay waterfall or public profile email | Clay | Per gtm-system.md step 8 |

## 4. Signals table [ENGINE]
| # | Signal | Role | How it's read | Output |
|---|---|---|---|---|
| 1 | Customer-facing job post, Lagos, ≤ 30 days | trigger | Regex on title (Section 2 pattern) AND location contains "Lagos" AND `today - posted_date ≤ 30` | boolean |
| 2 | Employer segment | router | Clay AI column on employer name + job description + Maps category. Fixed list: `s1_retail`, `s2_food`, `s3_salon`, `recruiter`, `clinic`, `other` | enum (only first three continue) |
| 3 | Channels the role covers | router | Regex on job description. Chat: `whatsapp\|instagram\|\bdm\b\|dms\|social media\|chat\|online order`. Calls: `\bcall\|calls\|phone\|telephone`. Chat only = `chat`; calls only = `calls`; both = `both`; neither = `not_stated` | enum: chat / calls / both / not_stated |
| 4 | Job title as posted | context-clause | Direct field | text → `{{role_title}}` |
| 5 | Takes orders in Instagram DMs (stacks with N1) | context-clause | Instagram account found AND a caption in last 14 days matches `dm to order\|send a dm\|whatsapp to order\|price in dm` | boolean |
| 6 | Running a Meta ad (stacks with U3) | context-clause | Advertiser matches employer AND an ad started ≤ 30 days ago | boolean |

Count clusters: no routing read is a count. The open-roles count is an exclusion filter (Section 2).

Deliberately excluded reads:
- Posted salary: not used in routing or copy. Copy built on it would pitch replacing a hire the owner just decided to make, and would trigger the job-loss objection in personas.md. Stored for analysis only.
- Review or follower counts: size proxy only (Section 2), not a router.

Routing axes for Section 5: Segment (3) x Role channel (4) = 12 cells. Approved by Fredrick 2026-11-13.

## 5. Routing matrix with angles [ENGINE]
PENDING. Next gate: build axis by axis, stop for approval.

## 6. Variables contract [ENGINE]
PENDING.

## 7. Cadence & channel [ENGINE]
PENDING.
