# Bad Strategy Detector — Rumelt's Four Hallmarks

## Purpose

This detector runs as an independent Haiku agent at every gate. It reads the phase output and flags any of the four hallmarks of bad strategy identified by Richard Rumelt.

## The Four Hallmarks

### 1. Fluff
**What it is**: Buzzwords, jargon, and inflated language that creates the illusion of high-level thinking without actual content.

**Detection signals**:
- Sentences that sound impressive but say nothing specific
- Jargon that could apply to any company/situation ("synergize," "paradigm shift," "transform")
- Restating the obvious in complex language
- Adjective-heavy descriptions with no concrete referents

**Flag format**: `FLUFF DETECTED: "[quoted text]" — This says [what it actually means / nothing]`

### 2. Failure to Face the Challenge
**What it is**: The analysis or strategy avoids naming the hard part of the problem. It talks around the difficulty rather than confronting it directly.

**Detection signals**:
- The core obstacle is never explicitly named
- The diagnosis describes symptoms but not root causes
- The strategy sidesteps the most difficult trade-off
- Risks are listed but the hardest one is missing or downplayed
- The analysis assumes away the central difficulty

**Flag format**: `CHALLENGE AVOIDANCE: The hard part is [X], but the analysis [describes what it does instead]`

### 3. Mistaking Goals for Strategy
**What it is**: Stating desired outcomes (grow revenue, increase engagement, win the market) without explaining the mechanism — the HOW.

**Detection signals**:
- Statements of ambition presented as strategy ("We will become the leader in...")
- Outcomes listed without causal logic connecting actions to results
- "Strategy" that is just a list of goals or KPIs
- No explanation of WHY these actions would produce these outcomes
- Guiding policy that describes WHERE we want to go, not HOW we'll get there

**Flag format**: `GOALS ≠ STRATEGY: "[quoted text]" is an outcome, not a mechanism. What action produces this result?`

### 4. Bad Strategic Objectives
**What it is**: Objectives that fail to address the core challenge, that are impractical, or that are a disconnected wish list.

**Detection signals**:
- Objectives that don't connect to the diagnosis
- A long list of objectives with no prioritization or sequencing
- Objectives that conflict with each other
- Objectives that are unmeasurable or unfalsifiable
- Objectives that assume unlimited resources

**Flag format**: `BAD OBJECTIVE: "[quoted text]" — [doesn't connect to diagnosis / conflicts with X / unmeasurable / assumes unlimited resources]`

## Agent Prompt (for Haiku)

```
You are a bad strategy detector. Read the following phase output and check for the four hallmarks of bad strategy:

1. FLUFF: Buzzwords masquerading as insight
2. CHALLENGE AVOIDANCE: Sidestepping the hard part
3. GOALS ≠ STRATEGY: Stating outcomes without mechanisms
4. BAD OBJECTIVES: Objectives disconnected from the core challenge

For each hallmark, output either:
- "PASS" if the output avoids this hallmark
- A specific flag with a quote and explanation if detected

Be ruthless. The point is to catch weak thinking before the PM sees it. If something is borderline, flag it — false positives are cheaper than false negatives.

Output format:
## Bad Strategy Check
- **Fluff**: [PASS or flag]
- **Facing the challenge**: [PASS or flag]
- **Goals vs strategy**: [PASS or flag]
- **Objective quality**: [PASS or flag]
```

## Integration

The bad strategy detector output is appended to each gate's TLDR. If ANY flag is raised:
- The flag text is shown to the PM
- Gate 2 at standard depth is NOT auto-approved (forces manual review)
- The team lead must acknowledge and address flags before the next phase
