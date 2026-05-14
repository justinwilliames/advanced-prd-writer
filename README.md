# advanced-prd-writer

A Claude Code skill for writing excellent product requirements documents — adaptive to the maturity of the work, opinionated about structure, and actively defensive against the ten failure modes that kill most PRDs.

This skill replaces "fill in a generic template" with a behaviour: read the request, pick the right document shape, enforce the small set of sections every credible PM authority agrees on, and scan the draft for the failure modes that quietly kill PRDs in review.

Built on synthesis of: Marty Cagan (SVPG), Lenny Rachitsky, Shreyas Doshi, Amazon Working Backwards, John Cutler, Ravi Mehta, Reforge, Aakash Gupta.

---

## What you get

**Seven document shapes**, picked adaptively from your request:

| Shape | When to use |
|---|---|
| Discovery brief | Problem not yet validated. Thinking out loud. |
| Opportunity assessment | Problem is real. Deciding go/no-go before staffing. |
| PR-FAQ | New product or major feature. Customer-visible win is the unknown. |
| Standard PRD | Validated problem. Ready to build. Multi-week scope. |
| Launch PRD | Building underway. Doc serves cross-functional GTM. |
| Technical RFC | Build path or architecture is the unknown. |
| Experiment brief | A/B or holdout test with a ship rule. |

**Eight universal must-haves**, enforced on every shape:

1. Problem statement with evidence
2. Target user / segment
3. Why now / opportunity
4. Success metrics with baseline, target, and dashboard
5. Solution overview (the what, not the how)
6. Non-goals / out of scope
7. Risks, assumptions, open questions
8. Owner, collaborators, timeline

**Ten failure modes**, actively detected on every draft:

1. Solution masquerading as problem
2. Unmeasurable success
3. No non-goals
4. Premature solutioning
5. Feature-named initiative
6. No customer evidence
7. Tradeoffs avoided
8. Length as quality proxy
9. No pre-mortem / no risks
10. Stale assumptions left implicit

**Two signature load-bearing patterns**:

- **Data-freshness callout** — every doc opens with version, date, and what data this is grounded in. Pre-empts the "is this stale?" question and forces honesty about provenance.
- **NEED / PROCEED-WITHOUT** — every dependency resolves as either a hard blocker (with owner and deadline) or a conscious cost we accept on launch. No "to be determined" rows.

---

## Install

### Claude Code CLI

Clone into your skills directory:

```bash
git clone https://github.com/justinwilliames/advanced-prd-writer.git ~/.claude/skills/advanced-prd-writer
```

Verify by listing skills inside any Claude Code session:

```
/skills
```

`advanced-prd-writer` should appear in the list.

### Claude Code Desktop (Mac / Windows)

Same as CLI — the Desktop app reads from `~/.claude/skills/`. Restart the app after cloning so it picks up the new skill.

### Manual install

If you'd rather not use git:

1. Download the repo as a zip.
2. Extract into `~/.claude/skills/advanced-prd-writer/`.
3. Confirm `~/.claude/skills/advanced-prd-writer/SKILL.md` exists.

---

## Usage

The skill triggers automatically whenever you ask Claude to write a PRD, spec, RFC, one-pager, press release, opportunity assessment, or experiment brief. You can also invoke it directly:

```
/advanced-prd-writer
```

### Example invocations

> "Write me a PRD for our new onboarding flow"

→ Standard PRD. The skill will pick this shape, load the template, ask one clarifying question if needed, then draft.

> "We're thinking about whether to add team workspaces — can you put something together?"

→ Discovery brief. Lighter shape. Focuses on whether the problem is real and what the next learning step is.

> "Write an RFC for migrating our auth from Cognito to Clerk"

→ Technical RFC. Adjudicates between engineering options. Recommendation with reasoning.

> "I want to test whether moving the trial CTA above the fold lifts signup"

→ Experiment brief. Falsifiable hypothesis, primary metric, ship rule decided up front.

> "If we built a feature that did X, what would the press release look like?"

→ PR-FAQ. Amazon Working Backwards shape.

The skill states its pick before drafting — *"Reading this as a Discovery brief — shout if you want a heavier shape."* You get one chance to redirect before any prose is written.

---

## What this skill does not do

- It does not produce marketing copy, support docs, or customer-facing prose. Different voice.
- It does not write engineering code. Hand off after the PRD is signed.
- It does not auto-fix the failure modes it detects. It surfaces and lets you decide.
- It does not pad. Length is failure mode #8. The skill cuts before it adds.

---

## Repo structure

```
advanced-prd-writer/
├── SKILL.md                    # entry point — Claude reads this on invocation
├── LICENSE                     # MIT
├── README.md                   # this file
├── templates/
│   ├── discovery-brief.md
│   ├── opportunity-assessment.md
│   ├── pr-faq.md
│   ├── standard-prd.md
│   ├── launch-prd.md
│   ├── technical-rfc.md
│   └── experiment-brief.md
└── references/
    ├── shape-picker.md         # full decision tree for picking the shape
    ├── failure-modes.md        # 10 failure modes, detection rules, fix patterns
    ├── pm-best-practices.md    # distilled wisdom from 8 PM authorities
    └── voice-clarity-rules.md  # writing rules with worked examples
```

`SKILL.md` is the entry point. `templates/` and `references/` are loaded on demand when the skill needs them.

---

## Voice and language

The skill writes in operator-direct, evidence-grounded prose. Short sentences. Active voice. No corporate hedging. Tables for facts. The voice rules are the same regardless of shape; the structure changes, not the register.

The skill defaults to the spelling convention your input uses. If unclear on the first draft, it asks once.

---

## Contributing

Issues and PRs welcome at [github.com/justinwilliames/advanced-prd-writer](https://github.com/justinwilliames/advanced-prd-writer).

If you have a PRD pattern that should be added, file an issue with:

- The pattern (one paragraph)
- The PM authority or real-world doc it's grounded in
- Which shape(s) it belongs in
- Whether it's a must-have, a should-have, or shape-specific

---

## Credits

Synthesis built on the published work of:

- **Marty Cagan** (Silicon Valley Product Group) — opportunity assessment, the discovery-over-documentation thesis
- **Lenny Rachitsky** — the 1-pager template, problem-statement primacy
- **Shreyas Doshi** — iterative PRDs, dashboard-view requirement, LNO framing
- **Amazon** — Working Backwards, the PR-FAQ structure
- **John Cutler** — one-pagers, outcomes over outputs, cost-of-delay
- **Ravi Mehta** — prototype-led specs, "the spec is the source code"
- **Reforge** — the 10-component product spec, 2–3 page discipline
- **Aakash Gupta** — modern PRD synthesis, evidence-grounded narrative

Author: Justin Williames. License: MIT.

---

## Changelog

- **v1.0** — Initial release. Seven shapes, eight must-haves, ten failure modes, two signature load-bearing patterns.
