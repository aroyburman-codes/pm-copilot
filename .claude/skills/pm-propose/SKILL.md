# PM Propose

Build a pitch document for a specific decision maker. Two modes: **Decision** (formal approval request) and **Steer** (discussion document seeking directional input).

## Invocation

```
/pm-propose [proposal description] --mode decision|steer --depth quick|standard|deep --context path --project name
```

**Defaults**: `--mode decision`, `--depth quick`

## Step 0: Parse Input and Setup

1. **Extract proposal** from free text. Identify the subject and the intended audience (decision maker).
2. **Parse flags**:
   - `--mode decision|steer` (default: decision)
   - `--depth quick|standard|deep` (default: quick)
   - `--context path/to/file.md` (optional prior output)
   - `--project name` (optional, auto-generate slug from input if omitted)
3. **Create output folder**: `./pm-workflow/{project-name}/`
4. **Auto-detect context**: Search `./pm-workflow/*/strategy-*.md`. If found, ask PM whether to use it:
   - "Found prior strategy output at [path]. Use as input context? (yes/no/specify different file)"
5. **Load references** — read these files for shared patterns. Resolve paths using Glob:
   - Find and read `gate-logic.md`, `bad-strategy-detector.md`, `depth-config.md`, `formatting.md` — search in the project's `shared/` directory or `.claude/skills/*/` directories
6. **Load role definitions** from `roles.md` in this skill folder.
7. **Load output templates** from `templates.md` in this skill folder.

---

## Quick Mode (default)

Single agent. No TeamCreate. Simulate all perspectives internally.

### Flow

1. **Frame the proposal**: Parse what is being pitched, to whom, and in which mode.
2. **Simulate skeptic perspectives** — work through each lens explicitly:
   - **Approver Lens**: How does this decision maker think? What do they reward? What concerns them?
   - **Alternatives**: What competes for the same resources? What is the opportunity cost?
   - **Feasibility**: Can this be delivered? What are the dependencies and timeline risks?
3. **Note tensions** between perspectives.
4. **Build Objection Response Matrix** (Decision mode) or **Trade-offs table** (Steer mode).
5. **Run bad strategy self-check** (inline, per `shared/bad-strategy-detector.md`).
6. **Draft the full document** using the appropriate template from `templates.md`.
7. **Save** to `./pm-workflow/{project-name}/propose-{project-name}.md`.
8. **Present Gate** via AskUserQuestion:

```
question: "Is this ready to send to the decision maker?"
options:
  - label: "Proceed"
    description: "Finalize and save the proposal."
  - label: "Redirect"
    description: "Adjust direction. You'll be asked for new guidance."
  - label: "Kill"
    description: "Stop. Findings so far are saved to the project folder."
```

Handle Redirect/Kill per `shared/gate-logic.md`.

---

## Standard Mode

Uses TeamCreate with agent teams, skeptic panels, and debate.

### Phase 1: Case — Build the Argument

**Goal**: Construct the core argument and anticipate objections.

**Team creation** via TeamCreate:

| Role | Model | Purpose |
|------|-------|---------|
| Team Lead | Opus | Argument construction, synthesis, orchestration |
| Approver Lens | Sonnet | See `roles.md` — decision-maker perspective |
| Alternatives | Sonnet | See `roles.md` — competing priorities, opportunity cost |
| Feasibility | Sonnet | See `roles.md` — deliverability, dependencies, risks |

Create tasks via TaskCreate to track progress through phases.

**Flow**:

1. **Team lead frames** the core argument based on input + any prior context.
2. **Skeptics analyze in parallel** — send each skeptic the framed argument and their role prompt from `roles.md`. Each produces a written position with evidence.
3. **Cross-pollination (1 round)**: Team lead shares ALL initial takes with ALL skeptics via SendMessage. Each skeptic reads peers' arguments and responds: challenges specific claims, strengthens or weakens own position, flags contradictions.
4. **Synthesis**: Team lead synthesizes the full debate into:
   - Areas of agreement
   - Unresolved tensions
   - Confidence-weighted conclusions
   - **Objection Response Matrix** (Decision mode) or **Trade-offs table** (Steer mode)
5. **Bad strategy check**: Spawn Haiku agent with the prompt from `shared/bad-strategy-detector.md`. Feed it the synthesis output. Collect flags independently.

**Gate 1 — Case Gate** via AskUserQuestion:

Present TLDR (3-5 bullets) + core argument summary + debate highlights + Objection Response Matrix draft + bad strategy flags.

```
question: "Is this argument sound enough to build a proposal on?"
options:
  - label: "Proceed"
    description: "Move to drafting the full proposal document."
  - label: "Redirect"
    description: "Adjust direction. You'll be asked for new guidance."
  - label: "Kill"
    description: "Stop. Findings so far are saved to the project folder."
```

### Phase 2: Draft — Write the Proposal

**Goal**: Draft the full proposal in the appropriate mode.

**Team**: Team Lead (Opus) + Writer (Sonnet). See `roles.md` for Writer role.

**Flow**:

1. **Team lead outlines** document structure based on mode:
   - Decision mode: use Decision template from `templates.md`
   - Steer mode: use Steer template from `templates.md`
2. **Writer drafts** each section. Mode shapes tone throughout:
   - Decision: assertive, complete, asks for approval
   - Steer: exploratory, frames questions, asks for input
3. **Team lead reviews** for argument coherence and mode-appropriate framing.
4. **Bad strategy check**: Haiku agent reviews the draft.

**Gate 2 — Draft Gate** (optional at standard depth):

Per `shared/gate-logic.md`, Gate 2 auto-proceeds UNLESS:
- Bad strategy detector raised flags
- Team lead flagged an explicit uncertainty
- Evidence quality is predominantly `[Blog/Opinion]` or `[Unverified]`

If Gate 2 fires:
```
question: "Is the framing right for your audience?"
options:
  - label: "Proceed"
    description: "Move to final polish."
  - label: "Redirect"
    description: "Adjust direction. You'll be asked for new guidance."
  - label: "Kill"
    description: "Stop. Draft saved to the project folder."
```

### Phase 3: Polish — Final Pass

**Goal**: Produce the final document, ready to send.

**Team**: Team Lead (Opus) does the final pass solo.

**Flow**:

1. **Final quality pass**: Check formatting per `shared/formatting.md` — bold lead-ins, source tags, tables, no filler.
2. **Mode consistency check**: Verify the entire document maintains the right tone for Decision or Steer mode.
3. **Save** to `./pm-workflow/{project-name}/propose-{project-name}.md`.

**Gate 3 — Polish Gate** via AskUserQuestion:

Present TLDR + final document summary + tone check + completeness check + bad strategy flags.

```
question: "Is this ready to send to the decision maker?"
options:
  - label: "Proceed"
    description: "Finalize. Document saved to project folder."
  - label: "Redirect"
    description: "Adjust direction. You'll be asked for new guidance."
  - label: "Kill"
    description: "Stop. Document saved as-is to the project folder."
```

Cleanup: TeamDelete after final gate resolution.

---

## Deep Mode

Same as standard with these differences:

### Phase 1: Case

- **4 skeptics** (add True Cost — see `roles.md`):

| Role | Model | Purpose |
|------|-------|---------|
| Team Lead | Opus | Argument construction, synthesis |
| Approver Lens | Sonnet | Decision-maker perspective |
| Alternatives | Sonnet | Competing priorities, opportunity cost |
| Feasibility | Sonnet | Deliverability, dependencies, risks |
| True Cost | Sonnet | Direct cost, opportunity cost, political capital cost, failure cost |

- **2 debate rounds**: After initial takes are shared, skeptics respond (round 1). Team lead shares round 1 responses, skeptics counter-respond (round 2). Then synthesis.

### Phase 2: Draft

- Gate 2 is **required** (not optional).

### Phase 3: Polish

- Same as standard.

---

## Mode Behavior Reference

Mode is not just a template switch. It affects tone and framing at every phase.

### Decision Mode

- **Tone**: Assertive, confident, complete. "We recommend X."
- **Argument**: Structured as a case to be approved or rejected.
- **Evidence**: Presented as supporting the recommendation.
- **Objections**: Addressed head-on in the Objection Response Matrix.
- **Ask**: Specific — what approval, resources, timeline, authority is needed.
- **Bad strategy detector**: Extra scrutiny on goals-vs-strategy (is the ask a real plan?).

### Steer Mode

- **Tone**: Exploratory, collaborative. "Here's where we're leaning."
- **Argument**: Structured as a thinking-in-progress document.
- **Evidence**: Presented as context for discussion.
- **Trade-offs**: Central artifact instead of Objection Response Matrix.
- **Questions**: Each open question states the current lean + what would change the team's mind.
- **Ask**: Specific directional input needed from the decision maker.

---

## Formatting Rules

Follow all rules from `shared/formatting.md`:

- **Bold lead-in** on every paragraph: `**Key point**: Supporting detail...`
- **Source quality tags** on all web data: `[Primary Source]`, `[Industry Report]`, `[Blog/Opinion]`, `[Unverified]`
- **Tables** for Objection Response Matrix, trade-offs, risk registers
- **TLDR** always first
- **No filler**, no buzzwords, no unsourced numbers

---

## Error Handling

- **Redirect**: Team lead re-synthesizes with PM's new direction. Skeptics do NOT re-run. If PM redirects twice on the same gate, suggest Kill + restart.
- **Kill**: Save all work to `./pm-workflow/{project-name}/`, add `_killed_at_gate_{N}` to filename, TeamDelete, report save location.
- **Missing context**: If no strategy output found and none provided, proceed with the proposal description as sole input. Note that findings would be stronger with upstream investigation/strategy work.
