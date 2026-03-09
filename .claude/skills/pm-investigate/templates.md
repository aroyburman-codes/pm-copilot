# PM Investigate — Output Templates

## Quick Mode Template

Use this condensed template when `--depth quick`.

```markdown
# Investigation: {Project Name}

## TLDR
- [Finding 1 — the headline conclusion]
- [Finding 2 — confidence level and why]
- [Finding 3 — biggest risk or gap]
- [Finding 4 — sizing headline if applicable]
- [Finding 5 — recommended next step]

## Hypothesis
**Original**: {PM's input verbatim}
**Refined**: {Testable version with claim, mechanism, and prediction}

## Analysis

### Strategic Lens
**{Bold lead-in summarizing the strategic take}**: {Analysis with source tags}

### Methodological Lens
**{Bold lead-in on testability}**: {Analysis with source tags}

### Builder Lens
**{Bold lead-in on feasibility}**: {Analysis with source tags}

### Data Lens
**{Bold lead-in on evidence availability}**: {Analysis with source tags}

### Tensions
**{Bold lead-in naming the key tension}**: {Where the lenses disagreed and why it matters}

---

## Sizing ({size_flag})
**{Bold lead-in with the headline number}**: {Calculation with methodology}

**Assumptions**:
| Assumption | Value | Source |
|------------|-------|--------|

**Range**: {Pessimistic} — **{Base}** — {Optimistic}

---

## Confidence Assessment
**Overall**: {High/Medium/Low} — {one sentence reasoning}
**Source quality**: {X primary, Y reports, Z blog/opinion, W unverified}

---

## Bad Strategy Check
- **Fluff**: {PASS or flag}
- **Facing the challenge**: {PASS or flag}
- **Goals vs strategy**: {PASS or flag}
- **Objective quality**: {PASS or flag}

---

## Recommendation
{Investigate further / Act on this / Abandon} — {reasoning in 2-3 sentences}
```

---

## Standard/Deep Mode Template

Use this full template when `--depth standard` or `--depth deep`.

```markdown
# Investigation: {Project Name}

## TLDR
- [Finding 1 — the headline conclusion]
- [Finding 2 — confidence level and why]
- [Finding 3 — biggest risk or gap]
- [Finding 4 — sizing headline]
- [Finding 5 — recommended next step]

---

## Hypothesis
**Original**: {PM's input verbatim}
**Refined**: {Testable version after Phase 1 debate}
**Claim**: {What specifically is being asserted}
**Mechanism**: {Why this would be true — the causal logic}
**Testable prediction**: {What observable evidence would confirm or refute this}

---

## Evidence

### Supporting Evidence
**{Bold lead-in for strongest supporting point}**: {Evidence with source tag}
[Source quality tag — source name and date]

**{Bold lead-in for second supporting point}**: {Evidence with source tag}
[Source quality tag — source name and date]

{Continue for all material supporting evidence}

### Counter-Evidence
**{Bold lead-in for strongest counter-point}**: {Evidence with source tag}
[Source quality tag — source name and date]

**{Bold lead-in for second counter-point}**: {Evidence with source tag}
[Source quality tag — source name and date]

{Continue for all material counter-evidence}

### Evidence Gaps
**{Bold lead-in naming the gap}**: {What we looked for but couldn't find, and why it matters}

{Continue for all significant gaps}

---

## Quantitative Analysis

### Methodology
**{Bold lead-in on approach}**: {What was measured and how}

### Sizing — {size_flag}

#### {Top-down / Bottom-up / Impact / Effort} Estimate
**{Bold lead-in with the number}**: {Calculation walkthrough}

```
{Key calculation shown explicitly — formula, inputs, result}
```

**Assumptions**:
| Assumption | Value Used | Source | Sensitivity |
|------------|-----------|--------|-------------|
| {assumption} | {value} | {source tag} | {what happens if 2x off} |

**Range**:
| Scenario | Value | Key Driver |
|----------|-------|------------|
| Pessimistic | {value} | {what's different} |
| **Base** | **{value}** | **{core assumptions}** |
| Optimistic | {value} | {what's different} |

### Charts
{If DS produced visualizations}
![{Chart description}](charts/{filename}.png)
**{Bold lead-in interpreting the chart}**: {What the chart shows and why it matters}

---

## Confidence Assessment
**Overall confidence**: {High/Medium/Low}
**{Bold lead-in with reasoning}**: {Why this confidence level, in 2-3 sentences}

**Source quality breakdown**:
| Source Type | Count | Key Sources |
|-------------|-------|-------------|
| Primary Source | {N} | {list} |
| Industry Report | {N} | {list} |
| Blog/Opinion | {N} | {list} |
| Unverified | {N} | {list} |

**Key assumptions that could change the conclusion**:
1. {Assumption}: If wrong, the conclusion shifts to {X}
2. {Assumption}: If wrong, the conclusion shifts to {X}
3. {Assumption}: If wrong, the conclusion shifts to {X}

---

## Debate Summary

### Phase 1: Explore
**{Bold lead-in naming the central tension}**: {What the skeptics disagreed about most}

**Agreements**: {Where all perspectives converged}

**Unresolved tensions**:
- **{Tension 1}**: {Skeptic A argued X because [evidence]. Skeptic B argued Y because [evidence]. Resolution: [how the team lead weighed them]}
- **{Tension 2}**: {Same format}

### How the Hypothesis Evolved
**{Bold lead-in on what changed}**: {How the original hunch was sharpened through debate, and what specifically changed}

---

## Objection Response Matrix

| Objection | Likelihood | Severity | Evidence | Response |
|-----------|-----------|----------|----------|----------|
| {Most likely objection} | High/Med/Low | High/Med/Low | {Source-tagged evidence} | {How to address it} |
| {Second objection} | High/Med/Low | High/Med/Low | {Source-tagged evidence} | {How to address it} |
| {Third objection} | High/Med/Low | High/Med/Low | {Source-tagged evidence} | {How to address it} |
| {Fourth objection} | High/Med/Low | High/Med/Low | {Source-tagged evidence} | {How to address it} |

---

## Bad Strategy Check
- **Fluff**: {PASS or specific flag with quote}
- **Facing the challenge**: {PASS or specific flag}
- **Goals vs strategy**: {PASS or specific flag}
- **Objective quality**: {PASS or specific flag}

{If any flags were raised}:
**Team lead response**: {How the flags were addressed in the final synthesis}

---

## Recommendation
**{Investigate further / Act on this / Abandon}**

**{Bold lead-in with the core reasoning}**: {2-3 sentences on why this recommendation}

**If acting**: {Suggested next steps — what to do with these findings}
**If investigating further**: {What specific questions remain and how to answer them}
**If abandoning**: {What we learned that's still useful}
```

---

## Gate Presentation Template

Use this format when presenting gates via AskUserQuestion. Follows `shared/gate-logic.md`.

### Gate 1 — Explore

```
TLDR:
- {Reframed hypothesis in one line}
- {Strongest supporting signal}
- {Strongest counter-signal}
- {Confidence level and primary driver}
- {Biggest open question}

KEY FINDINGS:
{2-3 paragraph summary of skeptic debate output. Bold lead-ins.}

BAD STRATEGY FLAGS:
{Output from Haiku detector — PASS or specific flags}

SUGGESTED REFINEMENTS:
- {If the hypothesis should be sharpened, how}
- {If scope should change, what direction}
```

Question: "Is this hypothesis sharp enough to investigate?"

### Gate 2 — Analyze

```
TLDR:
- {Methodology headline}
- {Top evidence finding with source quality}
- {Sizing headline number with range}
- {Biggest data gap or limitation}
- {Confidence in methodology}

EVIDENCE SUMMARY:
{Source quality distribution: N primary, N reports, N blog, N unverified}
{2-3 paragraph summary of research + DS output. Bold lead-ins.}

DS ANALYSIS HIGHLIGHTS:
{Key quantitative findings. Reference charts if produced.}

BAD STRATEGY FLAGS:
{Output from Haiku detector}
```

Question: "Is this methodology sound? Are we measuring the right things?"

### Gate 3 — Synthesize

```
TLDR:
- {Headline conclusion}
- {Confidence: High/Medium/Low with one-line reasoning}
- {Biggest risk to the conclusion}
- {Sizing summary}
- {Recommendation: Investigate further / Act / Abandon}

FINDINGS SUMMARY:
{3-4 paragraph summary of the complete investigation. Bold lead-ins.
Cover: what we found, how confident we are, what could change our mind.}

BAD STRATEGY FLAGS:
{Final detector output}
```

Question: "Do you trust this finding enough to act on it?"

---

## Partial Save Template (Kill)

When the PM kills at any gate, save work completed so far.

```markdown
# Investigation: {Project Name} [KILLED AT GATE {N}]

**Status**: Stopped by PM at Gate {N} ({gate_name})
**Date**: {timestamp}
**Hypothesis**: {Original input}

## Work Completed

### {Phase name completed}
{Include whatever output was produced before the kill}

### Gate {N} Output (at time of kill)
{The TLDR and findings that were presented when PM chose Kill}

## Notes
{Any PM redirect comments or kill reasoning if provided}
```

---

## Formatting Checklist

Before saving any output, verify:

- [ ] TLDR is the first section after the title
- [ ] Every body paragraph starts with a **bold lead-in**
- [ ] Every web-sourced data point has a source quality tag
- [ ] All tables have headers and aligned columns
- [ ] Sizing includes explicit assumptions, methodology, and a range
- [ ] Charts (if any) are saved to `charts/` and referenced with `![description](path)`
- [ ] No filler phrases, hedging without specificity, or buzzwords
- [ ] No unsourced quantitative claims
- [ ] Confidence levels use the standard scale (High/Medium/Low with definitions)
- [ ] Objection Response Matrix is present (standard/deep only)
- [ ] Bad Strategy Check section is present with all four dimensions
- [ ] Recommendation section has clear next steps
