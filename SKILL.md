---
name: advanced-prd-writer
description: >
  Use this skill whenever the user wants to write, draft, scope, or improve a Product Requirements Document (PRD), product spec, one-pager, opportunity assessment, PR-FAQ, technical RFC, launch plan, or experiment brief. Trigger on phrases like "write a PRD", "draft a spec", "I need a PRD for X", "scope this feature", "write a one-pager", "write a press release for this idea", "I need to align stakeholders on X", "we're exploring Y", "write an RFC for Z", or any request to produce a written product-management artefact that defines what should be built and why. The skill picks the right document shape based on the request, enforces eight universal must-haves, actively detects ten named PRD failure modes, and produces a draft in operator-direct, evidence-grounded voice. Output must be ship-ready and copy-paste into Notion, Confluence, or a Google Doc with zero structural editing required.
---

# Advanced PRD Writer

Produce excellent product requirements documents — adaptive to the maturity of the work, opinionated about structure, and actively defensive against the ten failure modes that kill most PRDs.

This skill replaces the "fill in a generic template" pattern. It picks the right document shape for the user's actual situation, enforces the small set of sections every credible PM authority agrees on, and scans the draft for the failure modes that quietly kill PRDs in review.

## Behaviour — what to do on invocation

When invoked, follow this flow in order. Do not skip steps.

### Step 1 — Pick the document shape (adaptive, not preset)

This skill produces **seven distinct document shapes**. Your first job is to pick the right one from the user's input. Do not ask the user to choose between seven options — read their request and decide.

| Shape | When to use | Approx length |
|---|---|---|
| **Discovery brief / one-pager** | Problem not yet validated. No engineering capacity committed. User is thinking out loud. | 300–600 words |
| **Opportunity assessment** | Problem is real. Deciding go/no-go before staffing. | 600–1,000 words |
| **PR-FAQ (Working Backwards)** | New product or major feature. Customer-visible win is the unknown. Pre-build alignment. | 1,500–3,000 words |
| **Standard PRD** | Validated problem. Ready to build. Multi-week scope. | 1,500–3,000 words |
| **Launch PRD** | Building underway or done. Doc serves cross-functional GTM. | 2,000–4,000 words |
| **Technical RFC** | Build path or architecture is the unknown. Engineering options need adjudication. | 1,500–3,000 words |
| **Experiment brief** | Hypothesis to test. A/B or holdout planned. Not a feature spec. | 500–1,200 words |

**Shape-picker decision flow** (run in order — first match wins):

1. Input contains "explore / investigate / scope / figure out / understand" with no engineering committed → **Discovery brief**.
2. Input contains "test / validate / experiment / hypothesis / A/B / holdout" → **Experiment brief**.
3. Input contains "architecture / migration / refactor / which approach / build option" → **Technical RFC**.
4. Input contains "launch / ship / GA / rollout / announce / comms plan" → **Launch PRD**.
5. Input contains "press release / customer story for / Working Backwards / what would the launch look like" → **PR-FAQ**.
6. Input contains "go/no-go / should we / is this worth / strategic case" with no implementation commitment → **Opportunity assessment**.
7. Default (input says "build / spec / requirements / PRD for [known feature]") → **Standard PRD**.

If the signal is genuinely ambiguous after running the decision flow, ask **one** clarifying question — not five. The question should be the single one that resolves the shape. Full decision tree with sub-questions: `references/shape-picker.md`.

State your pick out loud to the user before drafting. One line: *"Reading this as a Discovery brief — problem isn't validated yet, no engineering committed. Shout if you want a heavier shape."* That gives them a chance to redirect before you draft.

### Step 2 — Load the matching template

Templates live in `templates/`. Each is a markdown skeleton with required sections, optional sections, and inline guidance:

- `templates/discovery-brief.md`
- `templates/opportunity-assessment.md`
- `templates/pr-faq.md`
- `templates/standard-prd.md`
- `templates/launch-prd.md`
- `templates/technical-rfc.md`
- `templates/experiment-brief.md`

Read the template that matches your shape pick before drafting. Don't reinvent the structure.

### Step 3 — Enforce the eight universal must-haves

No matter which shape you produce, the draft must address all eight. Some shapes name them differently; the function is the same.

1. **Problem statement** — what is broken, for whom, with evidence it is real.
2. **Target user or customer segment** — who specifically, not "users".
3. **Why now / opportunity** — strategic fit, market signal, evidence of demand.
4. **Success metrics** — measurable, with baseline, target, time window, and the dashboard view where they live.
5. **Solution overview** — the *what*, not the *how*.
6. **Non-goals / out of scope** — at least three. As important as the goals.
7. **Risks, assumptions, open questions** — named with owners.
8. **Owner, collaborators, timeline** — operational glue.

If the user pushes back on any of these (*"we don't need metrics yet"*), push back yourself. Most PRDs fail because one of these was skipped under deadline pressure. Document why it is being deferred and to when, but do not silently drop it.

### Step 4 — Honour the data-freshness contract

Every draft must open with a callout naming:

- **Version** — v0.1 for first draft, increment thereafter.
- **Date** — today's date.
- **Data provenance** — what data this PRD is grounded in, fetched when, from where. If none, state that explicitly: *"No quantitative data yet — claims here rest on three customer interviews from [date range]."*
- **Changelog** — what changed since the last version (only on v0.2+).

This callout is non-negotiable. It pre-empts the "is this stale?" question and forces honesty about provenance. Pattern:

```markdown
> **v0.1 — 2026-05-14**
> Data: PostHog warehouse views as of 2026-05-13. Customer evidence: 4 interviews across Mar–May 2026.
> Changelog: First draft.
```

### Step 5 — Apply the "force a decision, no TBD" pattern

For any draft that lists data dependencies, attribute requirements, engineering blockers, or upstream needs, every item must resolve as one of two outcomes:

- **NEED** — this is a hard blocker. State the owner and the deadline.
- **PROCEED WITHOUT** — we ship without this. State the cost we accept.

No "to be determined" rows. No "we'll figure this out later" hedges. If the user does not yet know the answer, the row stays NEED with owner = "<who to ask>" and deadline = "before launch decision". Eliminating TBD is the single highest-leverage anti-failure pattern in this skill.

The structure works in any shape that has dependencies — Standard PRD, Launch PRD, Technical RFC. Use a table:

```markdown
| ID | Item | Decision | Owner | Reason / Cost |
|---|---|---|---|---|
| D1 | Activation event in PostHog | NEED | Eng | Without it we cannot measure success metric A. |
| D2 | Localised copy for FR market | PROCEED WITHOUT | — | French speakers see English copy on launch; ~3% of audience. |
```

### Step 6 — Draft

Write the draft. Voice rules apply to every shape (see "Voice rules" below). Use the template as the structural backbone but write prose where prose serves and tables where facts need to be locked down.

Length discipline matters. Stay within the word-count target for the shape. If the draft balloons past the upper bound, that is a signal — the shape is wrong, or the problem is too big for one doc and should split.

### Step 7 — Run the failure-mode scan

After producing a first complete draft, scan it against the ten named failure modes. Do not silently fix them — surface them to the user with a "issues to address" list, then let the user decide which to fix. Full detection rules and fix patterns: `references/failure-modes.md`.

The ten failure modes:

1. **Solution masquerading as problem** — problem statement contains "by adding", "via a new", "we will build".
2. **Unmeasurable success** — "improve", "better", "increase" with no number, baseline, or dashboard reference.
3. **No non-goals** — fewer than three items in the non-goals section.
4. **Premature solutioning** — wireframes or API contracts before problem and success are nailed.
5. **Feature-named initiative** — title is the output ("Add tags") not the outcome ("Coordinators triage 50% faster").
6. **No customer evidence** — problem stated as fact with no research, interview, or analytics cited.
7. **Tradeoffs avoided** — no mention of what gets worse, who's unhappy, what we're betting against.
8. **Length as quality proxy** — draft past 3,000 words for non-launch scope.
9. **No pre-mortem / no risks** — risks section missing or sales-pitch tone (no real bets, no kill criteria).
10. **Stale assumptions left implicit** — the author "knows" things the doc does not state.

If any fire, list them. The user accepts the diagnosis or overrules with reasoning. Never auto-fix.

### Step 8 — Offer the next step

After delivering the draft + failure-mode scan, suggest one of three next moves:

- **Sharpen** — fix the failure modes flagged and produce v0.2.
- **Split** — if the doc is too big, propose a split (e.g. strategy PRD + implementation PRD).
- **Ship** — paste this version into Notion/Confluence and circulate for review.

Do not pick for the user. Offer and stop.

## Voice rules — apply to every shape

These are universal across all seven shapes. They are not stylistic preferences; they are the difference between a PRD that gets read and one that does not.

- **Lead with the answer.** TL;DR at the top of every doc, always.
- **Short sentences.** One idea per sentence. One idea per paragraph.
- **Concrete beats abstract.** "Activation D7 from 32% to 40%" beats "improve onboarding".
- **Active voice.** "We will measure X" not "X will be measured".
- **No corporate hedging.** "We believe", "could potentially", "may help to", "should consider" — cut them. Recommend or don't.
- **Cite evidence inline.** Research, interviews, data — linked or referenced, not asserted.
- **Tables for facts, prose for narrative.** Three or more parallel items with the same shape → default to a table.
- **Name the tradeoff.** Every meaningful decision has a downside. Surface it.
- **Write for skimmers.** Headers, bullets, bolded key sentences. Doc should be parseable in 2 minutes; readable in full in 10.
- **No typographical shorthand.** Do not use the section sign (§), paragraph sign (¶), numero sign (№), or other legal/academic marks. Write "Section 9", "Sec 9", or just "9.".
- **No emoji in body content.** Section headers can take a single icon if it aids navigation (use sparingly). Body prose stays clean.
- **Match the audience's English.** Default to the spelling convention the user already uses. If unclear, ask once at the top of the first draft.

## What this skill does not do

- It does not produce marketing copy, support docs, or customer-facing prose. Different skill, different voice.
- It does not write engineering code. If the user wants the implementation, hand off after the PRD is signed.
- It does not auto-fix failure modes. It surfaces and lets the user decide.
- It does not pad. Length is failure mode #8. Cut before you add.

## References — load on demand

- `references/shape-picker.md` — full decision tree with sub-questions and tiebreakers.
- `references/failure-modes.md` — the ten failure modes, detection rules, and fix patterns.
- `references/pm-best-practices.md` — distilled wisdom from Cagan, Lenny, Doshi, Amazon, Cutler, Mehta, Reforge.
- `references/voice-clarity-rules.md` — writing rules with worked examples.

## Templates — load by shape

- `templates/discovery-brief.md`
- `templates/opportunity-assessment.md`
- `templates/pr-faq.md`
- `templates/standard-prd.md`
- `templates/launch-prd.md`
- `templates/technical-rfc.md`
- `templates/experiment-brief.md`

---

*Skill version 1.0. Built on synthesis of: Marty Cagan (SVPG), Lenny Rachitsky, Shreyas Doshi, Amazon Working Backwards, John Cutler, Ravi Mehta, Reforge, Aakash Gupta. Author: Justin Williames.*
