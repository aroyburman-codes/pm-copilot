# PM Propose — Output Templates

Two templates based on `--mode`. Mode shapes the entire document, not just the structure.

---

## Decision Mode Template

Use when the PM wants a formal proposal asking for yes/no/conditional approval.

```markdown
# Proposal: {Project Name}

## TLDR

- [3-5 bullets summarizing the proposal and the ask]
- [Final bullet is always the specific ask: "We're requesting X to do Y by Z"]

---

## Executive Summary

**The situation**: [1-2 sentences on what's happening and why it matters now]

**The proposal**: [1-2 sentences on what we want to do]

**The expected outcome**: [1-2 sentences on what success looks like, with timeline]

---

## The Case

**The challenge**: [What problem or opportunity we're responding to. Name the hard part directly — don't talk around it.]

**Our approach**: [How we'll address it. This must be a mechanism, not a goal. Explain WHY this approach works, not just WHAT we'll do.]

**Evidence**: [Key data points supporting the case. Each data point gets a source quality tag.]

- "[Data point]" [Primary Source — source name]
- "[Data point]" [Industry Report — source name]
- "[Data point]" [Blog/Opinion — source name]

**Expected impact**: [Quantified where possible. Include timeframe and assumptions.]

---

## Objection Response Matrix

| Objection | Likelihood | Severity | Evidence | Response |
|-----------|-----------|----------|----------|----------|
| [Most likely objection] | High/Med/Low | High/Med/Low | [What supports this concern] | [How we address it] |
| [Second objection] | | | | |
| [Third objection] | | | | |

Minimum 3 rows. Each objection should come from the skeptic panel's debate — these are real concerns, not strawmen.

---

## The Ask

**Approval**: [What decision we need — go/no-go, conditional approval, phased commitment]

**Resources**: [Headcount, budget, tooling — be specific]

**Timeline**: [Key dates, milestones, decision points]

**Authority**: [What permissions or organizational changes are needed]

---

## Alternatives Considered

For each alternative:

**[Alternative name]**: [1-2 sentence description]
- **Why it's viable**: [Honest assessment of its merits]
- **Why we didn't choose it**: [Specific reason, not "it's not as good"]

Include at minimum:
1. The status quo (do nothing)
2. One scaled-down version
3. One fundamentally different approach

---

## Risk and Mitigation

| Risk | Likelihood | Impact | Mitigation | Owner |
|------|-----------|--------|------------|-------|
| [Top risk] | High/Med/Low | High/Med/Low | [Specific mitigation action] | [Role/team] |
| [Second risk] | | | | |
| [Third risk] | | | | |

Top 3 risks minimum. Each mitigation must be a concrete action, not "we'll monitor this."

---

## Bad Strategy Check

- **Fluff**: [PASS or specific flag]
- **Facing the challenge**: [PASS or specific flag]
- **Goals vs strategy**: [PASS or specific flag]
- **Objective quality**: [PASS or specific flag]

---

## Appendix

### Supporting Data
[Full evidence table with all sources and quality tags]

### Methodology Notes
[How sizing was calculated, what assumptions were made]

### Debate Summary
[Key tensions from skeptic panel and how they resolved]
```

---

## Steer Mode Template

Use when the PM wants a discussion document seeking directional input, not approval.

```markdown
# Direction Check: {Project Name}

## TLDR

- [3-5 bullets summarizing the direction being explored]
- [Final bullet frames what input is needed: "We're looking for your read on X"]

---

## Context

**What we've found**: [Summary of the situation — what research, analysis, or strategy work led here. This is background, not argument.]

**Why this matters now**: [What makes this time-sensitive or important. Be specific about triggers.]

**How we got here**: [Brief chain of reasoning — investigation findings, strategy work, or new data that prompted this discussion.]

---

## Our Current Thinking

**Where we're leaning**: [State the direction clearly. "We're leaning toward X because Y."]

**Why this direction over others**: [Brief comparison, but framed as a lean, not a conclusion.]

**What we're less sure about**: [Honest statement of uncertainty. Name specific areas where the team isn't aligned or where evidence is thin.]

---

## Open Questions

For each question, provide the current lean and what would change the team's mind.

### 1. [Question that needs input]

**Our current lean**: [Where the team is leaning and why]

**What would change our mind**: [Specific data, argument, or constraint that would shift this]

### 2. [Question]

**Our current lean**: [...]

**What would change our mind**: [...]

### 3. [Question]

**Our current lean**: [...]

**What would change our mind**: [...]

Minimum 3 open questions. These should be genuine — not questions where the team has already decided but wants validation.

---

## Trade-offs We're Weighing

| Option | Upside | Downside | Our Lean |
|--------|--------|----------|----------|
| [Option A] | [Specific benefit] | [Specific cost or risk] | [Where we lean and why] |
| [Option B] | | | |
| [Option C] | | | |

Minimum 3 rows. Options should be genuinely distinct — not "good option" vs. "obviously worse option."

---

## What We'd Like From You

**Directional input**: [What specific direction or priorities do we need from the decision maker?]

**Concerns to surface**: [What worries should we know about that would change the calculus?]

**Constraints to confirm**: [What assumptions are we making about budget, timeline, or priorities that need validation?]

Be specific. "Your thoughts" is not an ask. "Whether you see the billing migration or this initiative as higher priority for the platform team in Q3" is an ask.

---

## Supporting Evidence

### Key Data Points

- "[Data point]" [Primary Source — source name]
- "[Data point]" [Industry Report — source name]
- "[Data point]" [Blog/Opinion — source name]

### Source Quality Summary

| Source Type | Count | Key Findings |
|-------------|-------|-------------|
| Primary Source | X | [Summary of what primary sources show] |
| Industry Report | X | [Summary] |
| Blog/Opinion | X | [Summary — note lower confidence] |
| Unverified | X | [Summary — flag explicitly] |

### Debate Summary
[Key tensions from skeptic panel and how they informed the current thinking]
```

---

## Formatting Rules (Both Templates)

These rules apply to all output regardless of mode. See `shared/formatting.md` for full reference.

### Bold Lead-Ins

Every body paragraph starts with a bold phrase summarizing the point:

```
**Market timing favors action now**: The competitive window narrows as three
major players announced similar initiatives in Q1... [Industry Report — Gartner 2025]
```

### Source Quality Tags

All web-sourced data gets tagged inline:

| Tag | Use When |
|-----|----------|
| `[Primary Source]` | Official company data, SEC filings, published research |
| `[Industry Report]` | Analyst reports, market research firms |
| `[Blog/Opinion]` | Blog posts, opinion pieces, social media |
| `[Unverified]` | Cannot confirm source reliability |

Format: `"claim text" [Tag — source name]`

### Tables

Use markdown tables for:
- Objection Response Matrix (Decision mode)
- Trade-offs (Steer mode)
- Risk registers
- Source quality summaries
- Any comparison with 3+ dimensions

### Anti-Patterns

- No filler phrases ("It's worth noting that...", "Interestingly...")
- No hedging without specificity ("This could potentially..." — state what conditions make it true)
- No buzzwords (see bad strategy detector)
- No walls of text — break into sections with bold lead-ins
- No unsourced quantitative claims — every number needs a source tag
- No strawman alternatives — each alternative must be genuinely viable

---

## Mode Tone Guide

| Aspect | Decision | Steer |
|--------|----------|-------|
| Voice | "We recommend" | "We're leaning toward" |
| Evidence framing | "The data supports" | "The data suggests" |
| Uncertainty | Addressed in risk section | Woven throughout as open questions |
| Central artifact | Objection Response Matrix | Trade-offs table + Open Questions |
| Closing | Specific ask for approval | Specific ask for input |
| Paragraph tone | Assertive, complete | Exploratory, inviting |
| Length target | Comprehensive — leave no question unanswered | Focused — enough context to have a good discussion |
