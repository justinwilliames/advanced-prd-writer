# [Outcome-phrased title, not feature-named — e.g. "Merchandisers update the seasonal catalog 50% faster — launch plan"]

> **v0.1 — [YYYY-MM-DD]**
> Launch target date: [YYYY-MM-DD]
> Data: [What this PRD is grounded in, fetched when, from where. E.g. "Warehouse views as of 2026-05-13. Customer evidence: 6 interviews across Mar–May 2026. Build status: feature-complete in staging as of 2026-05-12; QA pass 2 of 3 complete."]
> Changelog: First draft.

<!--
Launch PRD — building is underway or done. 2,000–4,000 words.
Core question this doc answers: "How does this land in the market?"
Reader: GTM, support, marketing, plus the engineering and design teams who built it. Decision it unblocks: launch can ship.

If this PRD will serve multiple cross-functional audiences (which is the default case for a Launch PRD), expand into the three-act spine:
  - Program — architecture, cadence, branching, eligibility logic
  - Content — copy, UX detail, module-level specification, all customer-facing strings
  - Data — instrumentation, attributes, events, schema, what we measure
The three-act spine lets the marketer read Section 1, the copywriter read Section 2, the engineer read Section 3, without anyone re-reading.
-->

## TL;DR

[Three lines. What we're shipping / who it's for / how we'll know it worked. Bold the launch date.]

[Example shape:
- **What:** Inline bulk-edit for product attributes on the seasonal catalog grid.
- **Who:** Pro and Enterprise plan merchandisers at mid-market e-commerce brands managing 5,000–50,000 SKUs.
- **When:** **Launching 2026-07-15** to 100% of Pro/Enterprise, with rollback criteria defined below.
- **Success:** Time-to-update p50 under 90s by launch + 30 days; weekly catalog-update completion up 4+ points by launch + 60 days.]

## Problem

[2–4 paragraphs. What is broken, for whom, with evidence inline. Cite the source for every claim.]

[Example: "Merchandisers at mid-market e-commerce brands lose 6–12 minutes per session switching between the product editor and a spreadsheet to bulk-update seasonal attributes — flagged in 4 of 6 Mar–May 2026 customer interviews. Warehouse `product_attribute_updated` p50 = 4m 18s vs the 30s workflow-spec target. Support tickets tagged `bulk-edit-friction` ran 42 in Q1, up from 18 in Q4."]

## Target user

[One paragraph. Specific segment, sized.]

[Example: "Merchandisers at mid-market e-commerce brands with 5,000–50,000 SKUs. ~340 organisations on the Pro plan match this profile, ~62% of Pro-plan MRR."]

## Why now

[One paragraph. Strategic fit + market timing + why this quarter.]

## Solution overview

[2–3 paragraphs. The WHAT, not the HOW. User-facing behaviour. Link to prototype: `[Figma — Inline bulk-edit v4](https://...)`.]

## Key user flows

[Step-by-step or link to prototype. Number each step.]

1. [Step.]
2. [Step.]
3. [Step.]

## Success metrics

[Table. Baseline + target + dashboard + owner per metric. Include launch + 7 day, launch + 30 day, launch + 90 day thresholds.]

| Metric | Baseline | Launch + 7d target | Launch + 30d target | Launch + 90d target | Dashboard | Owner |
|---|---|---|---|---|---|---|
| Median time-to-update | 4m 18s | <3m | <90s | <60s | Warehouse `merchandiser-workflow-health` | RevOps |
| Weekly catalog-update completion (Pro) | 32% | no regression | 34%+ | 38%+ | Warehouse `pro-catalog-funnel` | RevOps |
| Support tickets `bulk-edit-friction` | 42 / quarter | flat | <30 (annualised) | <20 (annualised) | CRM ticket dashboard | Support |
| Feature adoption (% of Pro orgs using ≥1x/week) | not tracked | establish | 60% | 80% | Warehouse `feature-adoption-merchandiser` | Product |
| Merchandiser NPS | not tracked | n/a | establish baseline | +10pt | Sprig `merchandiser-nps` | Product |

> guidance: launch metrics need a time window per row. Launch + 7d is the "did we break anything" check; + 30d is the early adoption read; + 90d is the real lift. Baseline can be "not tracked" — name it honestly.

## In scope

- [Item.]
- [Item.]
- [Item.]

## Out of scope (non-goals)

[Minimum 3 items.]

- [Non-goal.]
- [Non-goal.]
- [Non-goal.]

## Things NOT used by this launch

[Optional but recommended for launch PRDs. Pre-empts the "why didn't you use X?" question about specific systems, events, or surfaces.]

- [System / event / surface NOT used — and why. E.g. "In-app messaging — not used. Merchandiser flow is grid-native; in-app would require a separate SDK plumbing job and Pro merchandisers don't have the messaging surface open during catalog work."]
- [System / event / surface NOT used — and why.]

## Rabbit holes

- **[Rabbit hole 1]** — Tempting. Don't. [Reason.]
- **[Rabbit hole 2]** — Tempting. Don't. [Reason.]
- **[Rabbit hole 3]** — Tempting. Don't. [Reason.]

## Dependencies + decisions required before launch

[NEED / PROCEED-WITHOUT decision table. Every item resolves as one of two outcomes. No "TBD" rows.]

| ID | Item | Decision | Owner | Deadline | Reason / Cost |
|---|---|---|---|---|---|
| D1 | Product API supports `bulk_update` with `product_id[]` array | NEED | Lead Eng | 2026-06-15 | Without it, bulk-edit does not work — feature is non-functional. |
| D2 | Warehouse event `product_attribute_updated` includes `time_to_update` property | NEED | Data Eng | 2026-06-22 | Required to measure success metric 1. No launch-readout possible otherwise. |
| D3 | Localised copy for FR-market | PROCEED WITHOUT | — | — | French-market merchandisers see English copy on launch; ~3% of Pro audience. Add in Phase 2 if FR signups exceed 50/month. |
| D4 | Support runbook entry in help centre | NEED | Support Lead | 2026-07-08 | Without it, Tier 1 escalates everything to product. Capacity bottleneck on launch week. |
| D5 | Marketing changelog entry on example.com | NEED | Marketing Lead | launch day | Customer-facing comms requirement; ship-blocker for external announcement. |

> guidance: this table is non-negotiable for a Launch PRD. Every dependency resolves NEED or PROCEED-WITHOUT. If you don't know, the row stays NEED with owner = "<who to ask>" and deadline = "before launch decision".

## Launch checklist

[Per-team. Owners and dates. The checklist doubles as the launch-readiness review agenda — pass criteria is "every row is checked or has an explicit waiver".]

### Product / PM

| Item | Owner | Due | Status |
|---|---|---|---|
| Feature flag wired, default OFF | [Name] | [YYYY-MM-DD] | [ ] |
| Internal demo for leadership | [Name] | [YYYY-MM-DD] | [ ] |
| Launch readiness review held | [Name] | [YYYY-MM-DD] | [ ] |
| Rollback criteria signed off | [Name] | [YYYY-MM-DD] | [ ] |

### Engineering

| Item | Owner | Due | Status |
|---|---|---|---|
| Feature-complete in staging | [Name] | [YYYY-MM-DD] | [ ] |
| End-to-end test suite passing | [Name] | [YYYY-MM-DD] | [ ] |
| Performance test (p95 latency under target) | [Name] | [YYYY-MM-DD] | [ ] |
| Error rate < 0.5% in canary | [Name] | [YYYY-MM-DD] | [ ] |
| Observability dashboards live | [Name] | [YYYY-MM-DD] | [ ] |

### Design / UX

| Item | Owner | Due | Status |
|---|---|---|---|
| Final visual QA on production-like data | [Name] | [YYYY-MM-DD] | [ ] |
| Empty / loading / error states reviewed | [Name] | [YYYY-MM-DD] | [ ] |
| Accessibility pass (WCAG 2.1 AA) | [Name] | [YYYY-MM-DD] | [ ] |

### GTM / Marketing

| Item | Owner | Due | Status |
|---|---|---|---|
| Launch comms drafted (internal Slack + customer email) | [Name] | [YYYY-MM-DD] | [ ] |
| Customer email tested in [CRM tool], send list scoped | [Name] | [YYYY-MM-DD] | [ ] |
| Changelog entry written on example.com | [Name] | [YYYY-MM-DD] | [ ] |
| Sales enablement deck updated | [Name] | [YYYY-MM-DD] | [ ] |

### Support

| Item | Owner | Due | Status |
|---|---|---|---|
| Support runbook entry in help centre | [Name] | [YYYY-MM-DD] | [ ] |
| Tier 1 enablement session held | [Name] | [YYYY-MM-DD] | [ ] |
| Escalation path documented | [Name] | [YYYY-MM-DD] | [ ] |

## Comms plan

| Audience | Channel | Owner | Send / publish date | Status |
|---|---|---|---|---|
| Internal — all hands | Slack #announcements + Loom walkthrough | [Name] | Launch day, 09:00 local | [ ] |
| Internal — sales | Slack #sales + 15-min walkthrough | [Name] | Launch day - 2 | [ ] |
| Internal — support | Slack #support + runbook link | [Name] | Launch day - 5 | [ ] |
| External — customer | [CRM tool] email (segment: Pro + Enterprise) | [Name] | Launch day, 10:00 local | [ ] |
| External — public changelog | example.com/changelog | [Name] | Launch day | [ ] |
| External — press (if applicable) | [Outlet / blog] | [Name] | [Date] | [ ] |
| External — social | LinkedIn (founder), X (company) | [Name] | Launch day + 1 | [ ] |

## Support enablement

[Plain-language FAQ for the support team. What is this feature, who's it for, what to escalate, where the runbook is. The skill defaults to writing this section as if a Tier 1 support agent will read it cold.]

**What is it?**
[Two sentences. E.g. "A new way for merchandisers to bulk-edit product attributes directly in the catalog grid, instead of exporting to a spreadsheet. It applies edits across selected rows in a single transaction."]

**Who has access?**
[One sentence. E.g. "All Pro and Enterprise organisations from launch day. Free and Starter plans do not see this surface."]

**Common questions to expect**

| Question | Answer |
|---|---|
| "Why doesn't bulk-edit apply to my filtered rows?" | Filter must be applied before selecting rows. Merchandiser can re-apply filter or select rows manually. Both paths work. |
| "Can I revert to the old workflow?" | Yes — `Settings > Workflow > Catalog editor > Use legacy view`. Logged in the warehouse as `revert_to_legacy_view`. |
| "Does this work for variants?" | No, parent products only for v1. Variant-level bulk-edit coming in Phase 2 (target Q4 2026). |

**What to escalate**
- [Escalation case 1 — e.g. "Any bulk-edit writing wrong data to product rows → escalate to Eng immediately, attach `org_id` and `product_ids`."]
- [Escalation case 2.]

**Runbook location:** [Link to help centre runbook]

## Rollback criteria

[What would cause us to roll back, who decides, how. The kill-criteria the launch readiness review signs off on.]

- **Rollback trigger 1 — [Threshold].** [E.g. "Error rate on `product_attribute_updated` event exceeds 2% for >30 minutes."] Decision-maker: [Name]. Action: [E.g. "Toggle feature flag OFF for all orgs; fall back to legacy view; investigate before re-enabling."]
- **Rollback trigger 2 — [Threshold].** [E.g. "More than 5 customer-reported data-corruption tickets in launch + 24h window."] Decision-maker: [Name]. Action: [What we do.]
- **Rollback trigger 3 — [Threshold].** [E.g. "p95 latency on catalog grid load exceeds 3s sustained for 1h."] Decision-maker: [Name]. Action: [What we do.]

> guidance: rollback criteria need explicit numbers, named decision-makers, and a defined action. "We'll roll back if something goes wrong" is not a rollback plan.

## Post-launch monitoring

[Which dashboards, who watches them, alert thresholds. The first 7 days are the watch window.]

| Dashboard | Watcher | Check cadence | Alert threshold |
|---|---|---|---|
| Warehouse `merchandiser-workflow-health` | [Name] | Daily for first 7d, weekly after | Time-to-update p50 > 3m for 2 consecutive days |
| Sentry — catalog-grid errors | [Name] | Real-time alert | Error rate > 1% for 15 minutes |
| Support tickets tagged `bulk-edit-friction` | [Name] | Daily for first 14d | > 5 tickets / day |
| Launch email delivery + open | [Name] | Daily for first 3d | Delivery < 95% or open < 25% |

## Risks + mitigations

- **Risk 1 — [Name].** [Why it matters.] **Mitigation:** [What we'll do.]
- **Risk 2 — [Name].** [Why it matters.] **Mitigation:** [What we'll do.]
- **Risk 3 — [Name].** [Why it matters.] **Mitigation:** [What we'll do.]

## Phase 2 candidates

[Explicit "this is what comes next if launch succeeds". Not a commitment — a candidate list with conditional triggers.]

- [Candidate — trigger condition. E.g. "Variant-level bulk-edit — triggered if Pro adoption hits 60%+ at launch + 30d."]
- [Candidate — trigger condition.]
- [Candidate — trigger condition.]

## Open questions

1. [Question — owner — recommendation.]
2. [Question — owner — recommendation.]
3. [Question — owner — recommendation.]

## Owner + collaborators + timeline

| Role | Name |
|---|---|
| PRD owner | [Name] |
| Engineering lead | [Name] |
| Design lead | [Name] |
| Data / analytics | [Name] |
| QA | [Name] |
| Support lead | [Name] |
| Marketing / GTM lead | [Name] |
| Launch readiness review date | [YYYY-MM-DD] |
| Launch date | [YYYY-MM-DD] |

---

*Author: [Name]. Owner: [Name]. Last updated: [YYYY-MM-DD]. Source data: [Warehouse view / interview set / analytics query, with date]. Section owner: [team].*
