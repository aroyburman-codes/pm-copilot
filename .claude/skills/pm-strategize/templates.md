# Output Document Templates

## Full Template (Standard / Deep)

Save to `./pm-workflow/{project-name}/strategy-{project-name}.md`

```markdown
# Strategy: {Project Name}

**Depth**: {quick|standard|deep}
**Date**: {YYYY-MM-DD}
**Challenge**: {original input}
**Evidence base**: {path to investigate output, if used}

---

## TLDR

- {3-5 bullet summary of the strategy}
- {Each bullet should be concrete and actionable}
- {PM should get 80% of the value from this section alone}

---

## Diagnosis

**{One-sentence statement of the core challenge}**: {Expanded explanation of what is actually going on. This is not a description of the situation — it is a judgment about the central obstacle that must be overcome.}

**{Key force or dynamic}**: {Explanation of the force and its implications.}

**{Supporting evidence}**: {Data points with source quality tags.}

---

## Guiding Policy

**{One-sentence statement of the approach}**: {Explanation of HOW this policy addresses the diagnosed challenge. Must explain the mechanism, not just the desired outcome.}

**Why this approach over alternatives**:
- **{Alternative 1}**: {Why it was rejected — specific weakness relative to diagnosis.}
- **{Alternative 2}**: {Why it was rejected.}

**Competitive response analysis**: {How competitors are likely to react. Whether the policy survives their response.}

---

## Coherent Actions

Actions are sequenced and coordinated. Each implements the guiding policy.

### Action 1: {Action name}

- **What**: {Specific, concrete description}
- **Why**: {How this action implements the guiding policy}
- **Dependencies**: {What must be true or complete before this action}
- **Owner archetype**: {e.g., "Product lead", "Engineering team", "Partnerships"}
- **Success signal**: {How you know this action succeeded}

### Action 2: {Action name}

- **What**: {Description}
- **Why**: {Connection to policy}
- **Dependencies**: {Prerequisites}
- **Owner archetype**: {Role}
- **Success signal**: {Measure}

### Action 3: {Action name}

{Same structure. Include as many actions as the strategy requires. Typical range: 3-7.}

---

## Milestones

| Milestone | Actions Included | Timeline Signal | Success Criteria |
|-----------|-----------------|----------------|-----------------|
| {M1 name} | {Action 1, 2} | {e.g., "After MVP launch"} | {Measurable criteria} |
| {M2 name} | {Action 3} | {e.g., "Q2 post-launch"} | {Measurable criteria} |
| {M3 name} | {Action 4, 5} | {e.g., "When M1+M2 validated"} | {Measurable criteria} |

---

## Risk Register

| Risk | Likelihood | Impact | Mitigation | Owner |
|------|-----------|--------|------------|-------|
| {Risk description} | High/Med/Low | High/Med/Low | {Specific mitigation action} | {Role} |
| {Risk description} | High/Med/Low | High/Med/Low | {Mitigation} | {Role} |
| {Risk description} | High/Med/Low | High/Med/Low | {Mitigation} | {Role} |

---

## Not In Plan

Explicit choices about what this strategy does NOT pursue, and why.

- **{Thing not being done}**: {Why — connects back to guiding policy or resource constraint.}
- **{Thing not being done}**: {Why.}
- **{Thing not being done}**: {Why.}

---

## Debate Summary

**Phase 1 — Diagnose**:
- **Key agreement**: {What the panel converged on}
- **Key tension**: {Where the panel disagreed and how it resolved}
- **Unresolved**: {Any tensions carried forward}

**Phase 2 — Design**:
- **Key agreement**: {Convergence point}
- **Key tension**: {Disagreement and resolution}
- **Unresolved**: {Carried forward}

---

## Bad Strategy Self-Check

| Dimension | Status | Notes |
|-----------|--------|-------|
| Fluff | PASS / FLAG | {If flagged: specific quote and explanation} |
| Facing the challenge | PASS / FLAG | {If flagged: what hard part is being avoided} |
| Goals vs strategy | PASS / FLAG | {If flagged: which items are outcomes, not mechanisms} |
| Objective quality | PASS / FLAG | {If flagged: which objectives are disconnected or impractical} |
```

---

## Condensed Template (Quick Mode)

Save to `./pm-workflow/{project-name}/strategy-{project-name}.md`

```markdown
# Strategy: {Project Name}

**Depth**: quick
**Date**: {YYYY-MM-DD}
**Challenge**: {original input}

---

## TLDR

- {3-5 bullet summary}

---

## Diagnosis

**{Core challenge}**: {What is actually going on. 2-3 paragraphs max.}

---

## Guiding Policy

**{Approach}**: {HOW, not WHAT. 1-2 paragraphs.}

---

## Coherent Actions

| # | Action | Why | Dependencies | Success Signal |
|---|--------|-----|-------------|----------------|
| 1 | {Action} | {Connection to policy} | {Prerequisites} | {Measure} |
| 2 | {Action} | {Connection to policy} | {Prerequisites} | {Measure} |
| 3 | {Action} | {Connection to policy} | {Prerequisites} | {Measure} |

---

## Milestones

| Milestone | Actions | Timeline Signal | Success Criteria |
|-----------|---------|----------------|-----------------|
| {M1} | {1, 2} | {Signal} | {Criteria} |
| {M2} | {3} | {Signal} | {Criteria} |

---

## Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| {Risk} | H/M/L | H/M/L | {Mitigation} |
| {Risk} | H/M/L | H/M/L | {Mitigation} |

---

## Not In Plan

- **{Item}**: {Why not}
- **{Item}**: {Why not}

---

## Bad Strategy Self-Check

| Dimension | Status | Notes |
|-----------|--------|-------|
| Fluff | PASS / FLAG | |
| Facing the challenge | PASS / FLAG | |
| Goals vs strategy | PASS / FLAG | |
| Objective quality | PASS / FLAG | |
```

---

## Gate TLDR Format

Used at each gate presentation. Keep tight — PM reads this, not the full output.

```markdown
## Gate {N}: {Phase Name}

### TLDR
- {Bullet 1 — most important finding}
- {Bullet 2}
- {Bullet 3}

### Key Findings
**{Finding 1 lead-in}**: {Detail with source tags if applicable.}
**{Finding 2 lead-in}**: {Detail.}

### Bad Strategy Flags
{Output from Haiku detector — either "All checks passed" or specific flags}

### Open Questions
- {Unresolved question 1}
- {Unresolved question 2}
```

---

## Filename Conventions

| File | Pattern | Example |
|------|---------|---------|
| Final strategy | `strategy-{project}.md` | `strategy-ai-coding-market.md` |
| Killed partial | `strategy-{project}_killed_at_gate_{N}.md` | `strategy-ai-coding-market_killed_at_gate_1.md` |
| Charts (deep) | `charts/{description}.png` | `charts/market-size-sensitivity.png` |
