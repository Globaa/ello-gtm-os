# Ello GTM Context OS

## Company
Ello (https://ello.africa/): one handle (ello.africa/@business) customers call or chat from a browser, with an AI agent that answers, takes orders and books; Nigerian small businesses first; Free, ₦15,000 and ₦40,000 a month plans (source: context/docs/research/company.md).

## How this folder works
- `.claude/skills/`: reusable Claude skills for GTM tasks. Read when a task matches a skill.
- `context/icp/`: ideal customer profile, personas, signals. Read before targeting or list building.
  - `icp.md` (DRAFT): five-layer ICP, not-a-fit list, open questions.
  - `personas.md` (DRAFT): owner (economic buyer), customer-facing staff (champion), social/ads runner (influencer, hypothesis).
  - `signal-matrix.md` (DRAFT): segments x personas with angles, scored signals, top five to build, workflow tracker.
- `context/positioning/`: Read before writing any pitch or comparison. `value-proposition.md`, `positioning.md`, `competitors.md` (DRAFT).
- `context/messaging/`: Read before writing any copy. `messaging.md` (DRAFT), `facts.md` (APPROVED 2026-11-13; only source of claims for copy).
- `context/brand-kit/`: Read before writing or designing anything public. `brand.md` (voice and key phrases; no visual identity found).
- `context/docs/`: reference docs. Read for depth on how Ello sells.
  - `gtm-system.md` (DRAFT): the 12-step outbound system, tools, decisions, four-pillar check. Miro board linked at the top.
  - `gtm-system-flowchart.json` / `.svg`: `/miro-gtm-flowchart` spec and built layout for that board.
  - `case-studies.md`: no customer stories found yet; what to collect.
- `context/docs/research/`: raw, sourced research on Ello and its market. Read when a context doc needs a source.
  - `company.md`, `customers.md`, `competitors.md`, `market.md`, `signals.md`, `voice.md` (checked 2026-11-12).
- `knowledge-base/playbooks/`: repeatable how-tos and templates (imported). Read before running a workflow.
  - `copy-iron-laws.md`, `copy-frameworks.md`, `copy-bans.yaml`, `linkedin-dm-frameworks.md`, `signal-matrix-framework.md`, `workflow-anatomy.md`, `templates/facts-template.md`
  - `clay/`: `iron-laws.md`, `expert.md`, `formulas.md`, `prompts.md`, `actions-data-credits.md`
- `knowledge-base/learnings/`: what worked and what did not. Read before planning a new campaign. (empty)
- `meetings/`: meeting notes and transcripts. Read when a decision or request needs its origin. (empty)
- `output/campaigns/`: campaign specs, assets and copy. Write here; read to reuse past work.
  - `campaign-strategy.md` (DRAFT): 20 ranked campaign ideas with mother intent, tier, volume band, difficulty.
  - `hiring-customer-service-rep/workflow-spec.md` (COPY-APPROVED 2026-11-17): Workflow B, email only; 3 emails (days 0/3/8) approved.
  - `instagram-order-posts/workflow-spec.md` (COPY-APPROVED 2026-11-17): Workflow C, ideas #1 + #2 (Instagram DM-to-order and orders-open posts), 8 cells; 3 emails (days 0/3/8) + 1 manual Instagram DM approved.
  - `hiring-customer-service-rep/clay-build/`: Clay build guide (beginner, Parts 0 to 10) + swimlane flow (.png/.html/.json), from `/clay-build-plan`.
- `tools/`: scripts and tool configs (imported). Read when a task needs automation.
  - `rex/` (Clay client + `claylens/` Chrome extension, 42 files), `crawl4ai/README.md`, `agent-browser/README.md`
- `weekly-priorities/`: one file per ISO week (YYYY-Www.md). Read at the start of every session. Now: `2026-W46.md`.
- `work-log/`: one file per ISO week, timestamped entries. Write after every change. Now: `2026-W46.md`.
- Root: `README.md` (what this repo is), `.gitignore`, `.env` (empty, ignored).

## Rules
- Cite a source for every company fact.
- Never invent numbers.
- No em dashes in any copy.
- Ask before sending or publishing anything.
- Log every change in this week's work log (`work-log/YYYY-Www.md`).

## Skills
Imported from bolaji-ships/gtm-starter-kit (0e6d90b), in `.claude/skills/`. Run with `/<name>`.
- Strategy: `signal-matrix`, `campaign-strategy`, `build-workflow-spec`, `campaign-copy`, `miro-gtm-flowchart`
- Clay build: `clay-build-plan` (875f72f; beginner Clay build guide + swimlane flow PNG from an approved spec, writes instructions only), `campaign-contract`, `design-clay-table-json`, `import-clay-table`, `build-clay`, `prompt-builder`, `save-clay-schema`
- Clay ops: `campaign-qa`, `calculate-costs`, `workspace-cost-audit`, `export-tables`
- Research and email: `crawl4ai`, `agent-browser`, `inbox-placement-test`

## Tools
- Treg: paid data lookups and generative models (enrichment, SERP, social, scraping). Check the price before every call.
- Miro: GTM system diagrams and flow charts (used by `/miro-gtm-flowchart`).
- Notion: publish finished docs for the team. This repo stays the source of truth. Mirror of `context/` and `output/` (no raw research): [Ello GTM OS](https://app.notion.com/p/41762bcf06ca813cac60ce83bbfc14bc) (private draft, published 2026-11-13).
- Clay: company and people sourcing, enrichment and AI columns. Local helper: `tools/rex/`.
- HubSpot: CRM of record for accounts, contacts, deals and outbound activity.
- n8n: automation glue between signals, Clay, sequencer and HubSpot.
- Local: `tools/crawl4ai/` (site crawling), `tools/agent-browser/` (browser automation). Dependencies not installed yet.
