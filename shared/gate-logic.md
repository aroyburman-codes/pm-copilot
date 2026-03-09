# Gate Logic Reference

## Gate Presentation

At each gate, present findings to the PM using AskUserQuestion with exactly 3 options:

### Format
1. **Compute a TLDR**: 3-5 bullet points summarizing the phase findings
2. **Surface key open questions**: What the analysis flagged as uncertain or contentious
3. **Include bad strategy flags**: Output from the Haiku bad strategy detector (if any flags raised)
4. **Present the decision**:

```
AskUserQuestion:
  question: "[Gate-specific question — see below]"
  options:
    - label: "Proceed"
      description: "[What happens next if PM approves]"
    - label: "Redirect"
      description: "Adjust direction. You'll be asked for new guidance."
    - label: "Kill"
      description: "Stop this workflow. Findings so far are saved to the project folder."
```

### Gate-Specific Questions

**Investigate**:
- Gate 1 (Explore): "Is this hypothesis sharp enough to investigate?"
- Gate 2 (Analyze): "Is this methodology sound? Are we measuring the right things?"
- Gate 3 (Synthesize): "Do you trust this finding enough to act on it?"

**Strategize**:
- Gate 1 (Diagnose): "Is this the right diagnosis of the core challenge?"
- Gate 2 (Design): "Is this a real strategy, or just goals dressed up?"
- Gate 3 (Roadmap): "Is this roadmap executable? Ready to commit?"

**Propose**:
- Gate 1 (Case): "Is this argument sound enough to build a proposal on?"
- Gate 2 (Draft): "Is the framing right for your audience?"
- Gate 3 (Polish): "Is this ready to send to the decision maker?"

## Redirect Handling

When PM selects "Redirect":
1. Ask a follow-up question: "What should change? Provide your new direction."
2. Team lead re-synthesizes incorporating the PM's redirect input
3. Skeptics do NOT re-run (their analysis is preserved)
4. Re-present the gate with updated synthesis
5. If PM redirects a second time on the same gate, suggest Kill + restart

## Kill Handling

When PM selects "Kill":
1. Save all work completed so far to `./pm-workflow/{project-name}/`
2. Add a `_killed_at_gate_{N}.md` suffix to indicate where it stopped
3. Clean up the agent team (TeamDelete)
4. Report: "Workflow stopped at [gate]. Partial findings saved to [path]."

## Gate 2 Optional Logic (Standard Depth)

At standard depth, Gate 2 auto-proceeds UNLESS:
- The bad strategy detector raised any flags
- The team lead's synthesis includes an explicit uncertainty or concern
- Evidence quality is predominantly `[Blog/Opinion]` or `[Unverified]`

If any of these conditions are true, Gate 2 fires normally. Otherwise, log "Gate 2 auto-approved (no concerns flagged)" and proceed to Phase 3.
