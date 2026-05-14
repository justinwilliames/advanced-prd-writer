---
name: advanced-prd-writer
description: >
  Use this skill whenever the user wants to write, draft, scope, improve, OR critique a Product Requirements Document (PRD), product spec, one-pager, opportunity assessment, PR-FAQ, technical RFC, launch plan, or experiment brief. Trigger on phrases like "write a PRD", "draft a spec", "I need a PRD for X", "scope this feature", "write a one-pager", "write a press release for this idea", "we're exploring Y", "write an RFC for Z", AND on critique requests like "review this PRD", "audit this draft", "critique this spec", "what's wrong with this PRD", "make this PRD better", "tear this apart", "is this good enough to ship". Also auto-detect mode: if the user pastes a long structured document (500+ words, PRD-shaped headers like Problem / Success / Solution), offer critique mode without waiting for an explicit ask. The skill draws on published PM authorities (Cagan, Lenny Rachitsky, Doshi, Amazon Working Backwards, Cutler, Mehta, Reforge, Gupta) — not on any single author's house style. It picks the right document shape, enforces eight universal must-haves, actively detects ten named failure modes, and pushes back adversarially when a draft drifts from published best practice. Output must be ship-ready and copy-paste into Notion, Confluence, or a Google Doc with zero structural editing required.
---

# Advanced PRD Writer

A skill for writing and critiquing product requirements documents — grounded in **published PM best practice** (Cagan, Lenny Rachitsky, Doshi, Amazon Working Backwards, Cutler, Mehta, Reforge, Gupta), not in any single author's house style.

The skill operates in two modes:

- **Write mode** — produce a new PRD adapted to the maturity of the work.
- **Critique mode** — audit an existing PRD against published best practice and surface specific edits.

Both modes enforce the same eight universal must-haves and scan for the same ten failure modes. The skill is **adversarial by default** — when a draft conflicts with published best practice, it cites the authority and recommends the change. The user overrules consciously, not by default.

## Behaviour — what to do on invocation

### Step 0 — Pick the mode

Read the user's input. Decide before doing anything else:

| Trigger | Mode |
|---|---|
| User asks to *write / draft / scope / spec* a new doc | **Write mode** — proceed to Step 1 |
| User asks to *review / critique / audit / improve / tear apart / check* an existing doc | **Critique mode** — proceed to Step C1 |
| User pastes a document of 500+ words with PRD-shaped headers (Problem / Solution / Success / Non-goals / etc.) and does NOT explicitly ask to write something new | **Critique mode** — offer it: *"This looks like an existing PRD. I can audit it against published best practice and surface specific edits — want me to run the scan?"* Wait for confirmation before scanning. |
| Genuinely ambiguous | Ask one short question to pick the mode |

If the user is in write mode and the resulting draft will be reviewed later, they can invoke the skill again with the draft + "critique this" to enter critique mode on the output.

---

## WRITE MODE

### Step 1 — Pick the document shape (adaptive, not preset)

Seven shapes. Pick from the user's input — do not present a menu.

| Shape | When to use | Authority | Approx length |
|---|---|---|---|
| **Discovery brief / one-pager** | Problem not yet validated. No engineering capacity committed. | Lenny Rachitsky, John Cutler | 300–600 words |
| **Opportunity assessment** | Problem is real. Deciding go/no-go before staffing. | Marty Cagan | 600–1,000 words |
| **PR-FAQ** | New product or major feature. Customer-visible win is the unknown. | Amazon Working Backwards | 1,500–3,000 words |
| **Standard PRD** | Validated problem. Ready to build. Multi-week scope. | Lenny Rachitsky, Reforge | 1,500–3,000 words |
| **Launch PRD** | Build underway. Doc serves cross-functional GTM. | Reforge, Aakash Gupta | 2,000–4,000 words |
| **Technical RFC** | Build path or architecture is the unknown. | Engineering convention | 1,500–3,000 words |
| **Experiment brief** | Hypothesis to test. A/B or holdout planned. | Reforge experiments | 500–1,200 words |

Run the shape-picker decision flow (first match wins):

1. Input contains "explore / investigate / scope / figure out / understand" with no engineering committed → **Discovery brief**.
2. Input contains "test / validate / experiment / hypothesis / A/B / holdout" → **Experiment brief**.
3. Input contains "architecture / migration / refactor / which approach / build option" → **Technical RFC**.
4. Input contains "launch / ship / GA / rollout / announce / comms plan" → **Launch PRD**.
5. Input contains "press release / customer story for / Working Backwards" → **PR-FAQ**.
6. Input contains "go/no-go / should we / strategic case / make the case" without implementation commitment → **Opportunity assessment**.
7. Default — named feature without exploratory or experimental framing → **Standard PRD**.

If genuinely ambiguous after the flow, ask **one** clarifying question. Full decision tree with tiebreakers and worked examples: `references/shape-picker.md`.

State your pick out loud before drafting: *"Reading this as a Discovery brief — problem isn't validated yet. Shout if you want a heavier shape."*

### Step 2 — Load the matching template

Templates in `templates/` follow published external shapes — Lenny's flat one-pager, Cagan's opportunity assessment, Amazon's PR-FAQ structure, Reforge's 10 components. Read the template that matches your shape before drafting.

### Step 3 — Enforce the eight universal must-haves

No matter the shape, the draft must address all eight. Each must-have is cited to the authority that named it most clearly:

1. **Problem statement with evidence** — Lenny: *"nailing the problem statement is the single most important step"*. Amazon: *"start with the customer and work backwards"*.
2. **Target user / customer segment** — Amazon's PR-FAQ heading is literally *"for [segment]"*.
3. **Why now / opportunity** — Cagan's opportunity assessment requires this explicitly.
4. **Success metrics** — measurable, with baseline, target, time window, and the dashboard view. Doshi: *"most PRDs don't cover what the dashboard will look like"*.
5. **Solution overview — the what, not the how** — Cagan's first rule.
6. **Non-goals / out of scope** — Lenny + Kevin Yien at Square: *"as important as the goals"*. Shape Up calls this "Rabbit holes".
7. **Risks, assumptions, open questions** — Cutler's *"risks to mitigate"*. Amazon's internal FAQ. Doshi's pre-mortem.
8. **Owner, collaborators, timeline** — operational glue. Lenny names it explicitly.

If the user pushes back on any of these (*"we don't need metrics yet"*), push back yourself with the cited authority. Document why it is being deferred and to when — don't silently drop it.

### Step 4 — Apply two cross-shape patterns from published practice

These two patterns are applications of published principle, not the author's invention. Apply them on any shape where they fit:

**A. Cite evidence inline + provenance callout.** Every draft opens with a callout naming version, date, and what data this is grounded in. Cagan, Mehta, and Doshi all argue that PRDs without provenance produce *"vague specs that produce messy codebases"* (Mehta). If the doc has no data yet, state that explicitly:

```markdown
> **v0.1 — [today's date]**
> Data: [What this is grounded in. E.g. "5 customer interviews Mar–May 2026; warehouse view orders_30d as of 2026-05-13." If no quantitative data yet, name the qualitative basis: "4 customer interviews; no quantitative validation yet — to be added before v0.2."]
> Changelog: First draft. *(omit on v0.1; populate on v0.2+)*
```

**B. Force every dependency to a binary decision — no TBDs.** Doshi: *"forcing ourselves to write down our thinking enables consensus better than a meeting"*. Cagan: PRDs that defer decisions create *"silent coordination cost"*. Every dependency, attribute requirement, or upstream blocker resolves as one of two outcomes — **NEED IT** (hard blocker, named owner, named deadline) or **PROCEED WITHOUT** (we ship and accept this named cost):

```markdown
| ID | Item | Decision | Owner | Reason / Cost |
|---|---|---|---|---|
| D1 | Activation event in warehouse | NEED | Data Eng | Without it we cannot measure success metric A. Deadline: 2026-06-01. |
| D2 | Localised copy for FR market | PROCEED WITHOUT | — | French speakers see English on launch; ~3% of audience. Revisit Phase 2. |
```

No "to be determined" rows. If the answer isn't known, the row stays NEED with owner = *"<who to ask>"* and deadline = *"before launch decision"*.

### Step 5 — Draft

Write the draft. Voice rules apply (see below). Use the template as the structural backbone. Prose for narrative, tables for facts.

Length discipline matters. If the draft balloons past the upper bound, that is itself a failure mode (#8) — the shape is probably wrong, or the problem is too big for one doc and should split. Cagan, Doshi, Reforge all flag length-as-quality as a top failure mode.

### Step 6 — Run the failure-mode scan

After producing a first complete draft, scan it against the ten named failure modes. **Surface findings as a list with severity — do not silently fix them.** The user decides what to accept and what to reject.

The ten failure modes (full detection rules: `references/failure-modes.md`):

1. **Solution masquerading as problem** — "by adding", "via a new", "we will build" in problem statement.
2. **Unmeasurable success** — "improve", "better", "increase" with no number, baseline, or dashboard.
3. **No non-goals** — fewer than three items in the non-goals section.
4. **Premature solutioning** — wireframes or API contracts before problem and success are nailed (Cagan #1 critique).
5. **Feature-named initiative** — title is the output not the outcome (Cutler).
6. **No customer evidence** — problem stated as fact with no research/interview/data cited.
7. **Tradeoffs avoided** — no mention of what gets worse, who is unhappy, what we are betting against.
8. **Length as quality proxy** — draft past 3,000 words for non-launch scope (Reforge, Doshi).
9. **No pre-mortem / no risks** — risks section missing or reads as sales pitch (Amazon truth-seeking critique).
10. **Stale assumptions left implicit** — author "knows" things the doc does not state.

### Step 7 — Offer the next step

After the draft + scan, propose one of three moves: **Sharpen** (fix flagged modes, produce v0.2), **Split** (if too big, propose a split), **Ship** (paste into Notion and circulate). Do not pick for the user.

---

## CRITIQUE MODE

The skill enters this mode when the user explicitly asks for a critique OR pastes an existing PRD-shaped document. **Stance: adversarial.** When the draft conflicts with published best practice, cite the authority and recommend the change. Do not soften.

### Step C1 — Confirm the input and the shape

Briefly confirm what was pasted: *"I'm reading this as a [Standard PRD / Launch PRD / etc.] — ~[word count] words, [N] sections. Running a full audit now."* If the shape is unclear or the doc is mid-draft, ask one question.

### Step C2 — Run the four-part audit

Run all four passes. Capture findings as you go.

**Audit 1 — Eight must-haves.** For each of the eight, mark `present / missing / weak`. For each `missing` or `weak`, name what specifically is absent or vague, and cite the authority. Examples:

> *Missing: success metrics. Lenny: "how do we know if we've solved this problem?" Doshi: "include the dashboard view." This draft says 'improve activation' — no number, no baseline, no dashboard. Add: metric / baseline / target / dashboard / owner table.*

**Audit 2 — Ten failure modes.** Scan against the ten. For each fired finding: severity (blocker / major / minor / nit), location (section + line), the specific signal that fired the detector, and the recommended fix. Example:

> *Failure mode #1 fired (Solution masquerading as problem) — major — Problem section, paragraph 2. Signal: "by adding inline call-outcome capture" in the problem statement. Fix: rewrite as "[Segment] cannot [task] because [reason], which costs [evidence]."*

**Audit 3 — Voice and clarity.** Scan against the 14 voice rules (see `references/voice-clarity-rules.md`). Common findings to flag adversarially:
- Corporate hedges ("we believe", "could potentially", "may help to", "should consider")
- Passive voice in load-bearing claims
- Absent TL;DR at the top
- Tables where prose serves and prose where tables would lock down facts
- Length over 3,000 words for non-launch scope
- Section sign or paragraph sign (§ / ¶ / №)
- Mixed English conventions in one doc

**Audit 4 — Best-practice gap audit.** This is the audit that calls out drift from published canon. Patterns the skill is willing to flag:
- **Idiosyncratic section structures** — if the doc uses a section spine no published authority advocates (Program / Content / Data three-acts, or any other house-specific shape), flag it: *"This spine is not in Lenny's, Cagan's, Amazon's, or Reforge's templates. Consider whether the same content fits a published shape — Lenny's flat structure or Reforge's 10 components — for easier reader handoff."*
- **Length over 3,000 words for non-launch scope** — Reforge: *"product specs are typically 2–3 pages"*. Doshi: most PRD time is *"Overhead disguised as Leverage"*. Flag and suggest splitting.
- **"What's NOT used" or "Rabbit holes" sections missing** — Shape Up + Lenny + Square's Yien all say non-goals are as important as goals. Flag if absent.
- **No press-release framing where the win is customer-facing** — Amazon: write the press release first. If the doc is product-launch-shaped and the customer-facing win is buried below the fold, recommend either re-leading with the value statement or producing a separate PR-FAQ.
- **Implicit assumptions** — Cutler: *"operating assumptions"* should be explicit. Doshi: pre-mortem. If you can identify an assumption the author is making that the doc does not state, surface it.

### Step C3 — Reconcile and prioritise

Group findings:

- **Blockers** — must fix before ship (failed must-have; failure mode #1, #2, #6).
- **Majors** — should fix before ship (other failure modes; load-bearing voice violations).
- **Minors** — would improve the doc (voice nits, structural suggestions).
- **Nits** — typography, spelling, low-importance prose changes.

Drop nits unless trivial. Drop findings the user has explicitly accepted as out of scope.

### Step C4 — Deliver the audit

Format:

```markdown
## Audit summary

[1-2 lines: shape, word count, headline verdict — "ship after fixes" / "needs rework" / "ship as-is".]

## Blockers (N)

1. **[Finding name]** — [section / location]. **Cited authority:** [name + one-line quote or principle]. **Fix:** [specific edit]. **Before:** [quote]. **After:** [proposed].
2. ...

## Majors (N)

[Same format]

## Minors (N)

[Compact list]

## Nits (N)

[Compact list, only if trivial]

## Recommended next move

[Sharpen / Split / Ship — with the specific top-3 fixes to prioritise.]
```

### Step C5 — Offer to apply

After delivering the audit, offer: *"Want me to apply the blockers and majors and produce a v0.2?"* Do not auto-apply. Edits land only with explicit confirmation.

---

## Voice rules — apply to every shape and both modes

These are derived from the universal positions Doshi, Cutler, Norton, Mehta, Amazon, and Lenny share. Not stylistic preferences — the difference between a PRD that gets read and one that does not.

1. **Lead with the answer.** TL;DR at the top of every doc, always.
2. **Short sentences.** One idea per sentence. One idea per paragraph.
3. **Concrete beats abstract.** "Activation D7 from 32% to 40%" beats "improve onboarding".
4. **Active voice.** "We will measure X" not "X will be measured".
5. **No corporate hedging.** "We believe", "could potentially", "may help to", "should consider" — cut them. Recommend or don't.
6. **Cite evidence inline.** Research, interviews, data — linked or referenced, not asserted.
7. **Tables for facts, prose for narrative.** Three or more parallel items → table.
8. **Name the tradeoff.** Every meaningful decision has a downside. Surface it.
9. **Write for skimmers.** Headers, bullets, bolded key sentences. Parseable in 2 minutes; readable in 10.
10. **No typographical shorthand.** Do not use § (section sign), ¶ (paragraph sign), № (numero sign). Write "Section 9" or just "9.".
11. **No emoji in body content.** Section header icons OK if used sparingly. Body stays clean.
12. **Match the user's English convention.** If the user writes "behaviour" and "organisation", produce AU/UK spelling. If "behavior" and "organization", produce US. Do not impose either.
13. **Open with version + data-freshness provenance.** From Step 4.A above.
14. **Force decisions — no TBDs.** From Step 4.B above. NEED IT or PROCEED WITHOUT.

## What this skill does not do

- It does not produce marketing copy, support docs, or customer-facing prose.
- It does not write engineering code. Hand off after the PRD is signed.
- It does not auto-fix findings in critique mode. It surfaces and lets the user decide.
- It does not pad. Length is failure mode #8.
- It does not preserve any individual author's house style. Published external best practice is the canon.

## References — load on demand

- `references/shape-picker.md` — full decision tree with sub-questions, tiebreakers, worked examples.
- `references/failure-modes.md` — the ten failure modes, detection rules, mechanical scan checklist, fix patterns.
- `references/pm-best-practices.md` — distilled wisdom from the eight authorities.
- `references/voice-clarity-rules.md` — writing rules with worked examples.

## Templates — load by shape

- `templates/discovery-brief.md` — Lenny + Cutler one-pager
- `templates/opportunity-assessment.md` — Cagan
- `templates/pr-faq.md` — Amazon Working Backwards
- `templates/standard-prd.md` — Lenny + Reforge
- `templates/launch-prd.md` — Reforge + Aakash Gupta
- `templates/technical-rfc.md` — engineering convention
- `templates/experiment-brief.md` — Reforge experiments

---

*Skill version 1.1. Canon: Marty Cagan (SVPG), Lenny Rachitsky, Shreyas Doshi, Amazon Working Backwards, John Cutler, Ravi Mehta, Reforge, Aakash Gupta. Maintainer: Justin Williames.*
