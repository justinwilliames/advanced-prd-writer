# [Outcome-phrased title, not feature-named — e.g. "Sales reps triage their pipeline 50% faster"]

> **v0.1 — [YYYY-MM-DD]**
> Data: [What this PRD is grounded in, fetched when, from where. E.g. "Warehouse views `warehouse.crm.deal_stage_transitions` and `warehouse.events.rep_pipeline_actions` as of 2026-05-13. Customer evidence: 6 interviews across Mar–May 2026. Support ticket sample: 42 tickets tagged `pipeline-friction` from Q1 2026."]
> Changelog: First draft.

<!--
Standard PRD — the workhorse. 1,500–3,000 words.
Core question this doc answers: "What are we building, and how do we know it works?"
Reader: engineering, design, QA. Decision it unblocks: build can start.

If this PRD will serve multiple cross-functional audiences (marketer reads X, copywriter reads Y, engineer reads Z), consider expanding the body into a three-act spine:
  - Program — architecture, cadence, branching, eligibility logic
  - Content — copy, UX detail, module-level specification
  - Data — instrumentation, attributes, events, schema
Default to the flat structure below unless the cross-functional case is real.
-->

## TL;DR

[Three lines. Problem / solution / success. One sentence each. Bold the lift you're claiming.]

[Example shape:
- **Problem:** Sales reps lose 6–12 minutes per shift switching between their CRM and the dialer to log deal-stage transitions.
- **Solution:** Inline deal-stage capture in the deal row, with auto-fill from the dialer webhook payload.
- **Success:** Time-to-log down from 4m 18s median to under 90s, lifting weekly deal-stage update completeness from 32% to 38% within 8 weeks of launch.]

## Problem

[2–4 paragraphs. What is broken, for whom, with evidence inline. Cite the source — interview, warehouse query, ticket count — for every claim. Real problems describe user pain or business gap, never the proposed fix.]

[Example: "Sales reps at mid-market SaaS teams lose 6–12 minutes per shift switching between Salesforce and their dialer to log deal-stage transitions — flagged in 4 of 6 Mar–May 2026 customer interviews as their top daily friction. Warehouse event `deal_stage_logged` (90-day window) shows a median time-to-log of 4m 18s against the 30s target in the workflow spec. Support tickets tagged `pipeline-friction` ran 42 in Q1, up from 18 in Q4 — a 2.3x increase as deal volume scaled with the Pro-plan customer base."]

> guidance: if you wrote "by adding", "via a new", or "we will build" in this section, that's solution masquerading as problem. Rewrite to "[Segment] cannot [task] because [reason], which costs [evidence]."

## Target user

[One paragraph. Specific segment, not "users". Named role, named context, named scale. Note the segment size if you know it.]

[Example: "Sales reps at mid-market SaaS teams with 5–25 reps per pod, who log 30+ deal-stage transitions per day and own the pipeline-hygiene workflow. ~340 organisations on the Pro plan match this profile (warehouse view `warehouse.crm.org_segment`, May 2026), representing ~62% of Pro-plan MRR."]

## Why now

[One paragraph. Strategic fit, market signal, evidence of demand. Why this quarter, not next.]

[Example: "Pro-plan churn rose from 4.1% to 6.8% month-over-month between Feb and Apr 2026, with exit-interview notes citing 'too much manual data entry' in 7 of 12 churn calls. The Q2 OKR commits Pro-plan net retention to 95%; fixing the dominant friction point in the rep workflow is the highest-leverage move available before the July board readout."]

## Solution overview

[2–3 paragraphs. The WHAT, not the HOW. Describe the user-facing behaviour, the moments of value, and what changes for the operator. Avoid implementation detail — that belongs in the engineering follow-up.]

[Link to prototype if one exists: `[Figma — Inline deal-stage capture v4](https://...)`. A working prototype beats prose for "what does it look like".]

> guidance: if you find yourself writing API contracts, data models, or wireframe-level detail in this section, you're solutioning prematurely. Hold the line — the *what*, not the *how*.

## Key user flows

[Step-by-step, or link to prototype. Number each step. Note the inputs, the system response, and the visible state change.]

1. [Step — input — system response — visible change. E.g. "Rep ends a dialer call — system auto-creates the next-action row in the CRM with deal ID, outcome, and `deal_status: unresolved` — row appears in the pipeline view within 2 seconds."]
2. [Step.]
3. [Step.]
4. [Step.]

## Success metrics

[Table format. Baseline + target + dashboard + owner per metric. Baseline can be "not tracked" — that's honest signal that the instrumentation conversation is now part of scope.]

| Metric | Baseline | Target | Time window | Dashboard | Owner |
|---|---|---|---|---|---|
| Median time-to-log (deal-stage transition) | 4m 18s (warehouse `deal_stage_logged` p50, 90d) | <90s | 8 weeks post-launch | Warehouse dashboard `rep-workflow-health` | RevOps |
| Weekly deal-stage update completeness (Pro plan) | 32% (warehouse `warehouse.crm.update_completeness`) | 38% | 12 weeks post-launch | Warehouse `pro-pipeline-hygiene` | RevOps |
| Support tickets tagged `pipeline-friction` | 42 / quarter | <20 / quarter | Q3 2026 | CRM ticket dashboard | Support |
| Rep NPS (in-app survey) | not tracked | establish baseline + 10pt lift | 12 weeks post-launch | Sprig dashboard `rep-nps` | Product |

> guidance: "improve", "better", "increase" without a number, baseline, or dashboard is unmeasurable success — failure mode 2. Every row needs all five columns. If a baseline is genuinely unknown, write "not tracked" — that surfaces the instrumentation work honestly.

## In scope

[Bulleted list. What this PRD covers. Be specific.]

- [Scope item — e.g. "Inline deal-stage capture for outbound dialer events on the deal-row pipeline view."]
- [Scope item.]
- [Scope item.]

## Out of scope (non-goals)

[Minimum 3 items. As load-bearing as the goals. Pre-empts scope creep and the "why didn't you do X?" question.]

- [Non-goal — e.g. "Not redesigning the CRM deal-row layout. Surface only, no IA change."]
- [Non-goal — e.g. "Not extending to inbound calls. Outbound pipeline only for v1."]
- [Non-goal — e.g. "Not building a rep-side analytics view. Reporting stays in the warehouse."]
- [Non-goal — e.g. "Not a Free-plan feature. Pro and Enterprise only."]

> guidance: fewer than three non-goals is failure mode 3. The section is doing real work — naming the lines we're choosing not to cross.

## Rabbit holes

[Obvious-but-wrong adjacent work the team might wander into. Name each one, then say "tempting — don't" with the reason. Imported from Shape Up; concentrates editorial judgment in one place.]

- **[Rabbit hole 1]** — Tempting. Don't. [Reason. E.g. "Building a generic 'logged events' framework instead of solving the deal-stage case. The framework adds 4–6 weeks of scope and we don't have a second use case yet."]
- **[Rabbit hole 2]** — Tempting. Don't. [Reason.]
- **[Rabbit hole 3]** — Tempting. Don't. [Reason. E.g. "Adding a 'mark as spam' option to the deal row. Real ask but unrelated to the pipeline-time problem; ship separately if at all."]

## Dependencies + decisions required before launch

[Use the NEED / PROCEED-WITHOUT decision table. Every item resolves as one of two outcomes. No "TBD" rows.]

| ID | Item | Decision | Owner | Deadline | Reason / Cost |
|---|---|---|---|---|---|
| D1 | Dialer webhook payload includes `deal_id` field | NEED | Lead Eng | 2026-06-01 | Without it we cannot auto-fill the deal row — feature does not work. |
| D2 | Warehouse event `deal_stage_logged` includes `time_to_log` property | NEED | Data Eng | 2026-06-08 | Required to measure success metric 1. No baseline visibility otherwise. |
| D3 | Localised copy for FR-market reps | PROCEED WITHOUT | — | — | French speakers see English copy on launch; ~3% of Pro audience. Add in Phase 2 if Pro FR signups exceed 50/month. |
| D4 | CRM custom property `deal_stage_outcome` exposed via API | NEED | Lead Eng | confirm before scoping freeze | Without it the deal row cannot persist the outcome. Needs answer before 2026-05-28 scoping review. |

> guidance: this is the single highest-leverage anti-failure pattern in this skill. No "TBD" rows. If you don't know yet, the row stays NEED with owner = "<who to ask>" and deadline = "before scoping freeze". Force the decision.

## Risks + mitigations

[Minimum 3 named risks with explicit mitigations. Doc reads like sales material if this section is missing — failure mode 9.]

- **Risk 1 — [Name].** [Why it matters.] **Mitigation:** [What we'll do to prevent or contain it.] [Example: "Dialer webhook delivery is occasionally delayed >30s under load — would break the auto-fill UX. Mitigation: add a 60s polling fallback on the deal-row view; surface a `Sync now` button if no event has landed within 90s."]
- **Risk 2 — [Name].** [Why it matters.] **Mitigation:** [What we'll do.]
- **Risk 3 — [Name].** [Why it matters.] **Mitigation:** [What we'll do.]

> guidance: name the tradeoff explicitly — what gets worse, who's unhappy, what we're betting against. Failure mode 7 is avoidable here.

## Phasing

[Only include if the work splits across multiple releases. Otherwise delete this section. List MVP, then phase 2+, with explicit dependencies between phases.]

**MVP (target ship: [date])**
- [Item — e.g. "Inline deal-stage capture for outbound dialer events. Pro plan only. EN copy only."]
- [Item.]

**Phase 2 (target ship: [date], conditional on MVP success metric 1 hitting target)**
- [Item — e.g. "Inbound deal-stage capture. Adds ~2 weeks scope; depends on dialer inbound webhook spec landing."]
- [Item.]

**Phase 3 (candidate, not committed)**
- [Item — e.g. "Rep-side analytics view. Only if Phase 2 metric uplift sustains 8+ weeks."]

## Open questions

[Numbered. Owner-tagged. State a recommendation where you have one. Aggregate the inline open items here; don't leave them scattered.]

1. [Question — owner — recommendation. E.g. "Should the auto-fill be reversible (i.e. rep can revert to manual entry)? Owner: PM. Recommendation: yes, with a single-click revert — protects against bad payloads without adding modal-level friction."]
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
| Target build start | [YYYY-MM-DD] |
| Target ship | [YYYY-MM-DD] |

---

*Author: [Name]. Owner: [Name]. Last updated: [YYYY-MM-DD]. Source data: [Warehouse view / interview set / analytics query, with date]. Section owner: [team].*
