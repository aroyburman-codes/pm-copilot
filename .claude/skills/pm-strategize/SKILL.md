# PM Strategize

Build a strategy and roadmap from a strategic challenge using structured diagnosis, skeptic debate, and human-gated phases.

## Input Parsing

Extract from user input:
- **Challenge**: The strategic challenge or opportunity (free text after `/pm-strategize`)
- **--depth**: `quick` (default) | `standard` | `deep`
- **--context**: Path to a prior output file to use as evidence base
- **--project**: Folder name override (auto-generated from challenge text if omitted)

Generate `project-name` from the challenge: lowercase, hyphenated, max 4 words.
Example: "Enter the AI coding market" -> `ai-coding-market`

---

## Setup

1. **Create output folder**: `./pm-workflow/{project-name}/`
2. **Auto-detect prior work**: Check `./pm-workflow/*/investigate-*.md` for existing investigation outputs. If found, ask the PM whether to use them as evidence base. If `--context` is provided, use that file directly.
3. **Read supporting files** before starting. Resolve paths relative to the skill directory using Glob to find them:
   - `rumelt.md` (in this skill folder) — the Good Strategy Kernel reference
   - `roles.md` (in this skill folder) — skeptic and specialist role definitions
   - `templates.md` (in this skill folder) — output document templates
   - Find and read `gate-logic.md`, `bad-strategy-detector.md`, `depth-config.md`, `formatting.md` — search in the project's `shared/` directory or `.claude/skills/*/` directories

---

## Quick Mode (default)

Single agent. No TeamCreate. Simulate all perspectives internally.

### Flow

1. **Frame the challenge**: Parse the input. If prior investigate output exists, incorporate as evidence.
2. **Diagnosis** (simulate Investor/Lead + PM Skeptic + Builder lenses):
   - What is actually going on? Name the core obstacle.
   - Consider each skeptic lens and note tensions between them.
   - Run a bad strategy self-check on the diagnosis.
3. **Design** (simulate Strategy Analyst + Execution Analyst lenses):
   - Draft a guiding policy: HOW to overcome the diagnosed challenge.
   - Define coherent actions: coordinated, specific, sequenced.
   - Self-check: "Are these actions or just goals?"
4. **Roadmap**: Sequence actions into milestones. Add risk register. Declare what is NOT in plan.
5. **Single gate**: Present combined TLDR + diagnosis + strategy + roadmap + bad strategy flags.

### Output
Save condensed document to `./pm-workflow/{project-name}/strategy-{project-name}.md` using the template from `templates.md`. Use the condensed (quick) variant.

### Gate
Present via AskUserQuestion per `shared/gate-logic.md`:
- Question: "Is this strategy sharp enough to act on? Diagnosis accurate, policy clear, actions concrete?"
- Options: Proceed / Redirect / Kill
- On Redirect: re-synthesize with PM's new direction, re-present gate.
- On Kill: save partial output, report path.

---

## Standard Mode (--depth standard)

### Team Setup

Use TeamCreate to create the team (e.g., `team_name: "strategize-{project-name}"`). Then spawn each teammate using the Agent tool with the `team_name` parameter and a descriptive `name`. Load role definitions from `roles.md` and include them in each teammate's spawn prompt.

```
TeamCreate: team_name="strategize-{project}"
Agent: name="investor-skeptic", team_name="strategize-{project}", prompt=[from roles.md]
Agent: name="pm-skeptic", team_name="strategize-{project}", prompt=[from roles.md]
Agent: name="builder-skeptic", team_name="strategize-{project}", prompt=[from roles.md]
```

Deep mode adds: `Agent: name="data-scientist", team_name="strategize-{project}", prompt=[from roles.md]`

- **Team Lead** (Opus): Orchestration, Rumelt kernel enforcement, synthesis, gates
- **Skeptics and specialists** are defined in `roles.md`

### Phase 1: Diagnose

**Goal**: Frame the strategic challenge accurately using the Rumelt diagnosis.

**Skeptic panel** (3 agents — see `roles.md` for full definitions):
- Investor/Lead
- PM Skeptic
- Builder

**Flow**:
1. Team lead frames the strategic situation. If prior investigate output exists, distribute it as evidence.
2. **Stage 1 — Initial Takes**: Send the framing to all 3 skeptics in parallel. Each analyzes through their lens and produces a written position with evidence. Use WebSearch (5-8 calls per agent) for research.
3. **Stage 2 — Debate (1 round)**: Team lead shares ALL initial takes with ALL skeptics via SendMessage. Each skeptic reads peers' arguments and responds: challenges specific claims, strengthens or weakens their own position, flags contradictions.
4. **Stage 3 — Synthesis**: Team lead synthesizes the full debate. Captures: agreements, unresolved tensions, confidence-weighted conclusions. Produces the Rumelt DIAGNOSIS: what is actually going on, what is the core challenge.
5. **Bad strategy check**: Run the Haiku bad strategy detector (see `shared/bad-strategy-detector.md`) on the synthesis. Is the diagnosis facing the real challenge?

**Gate 1 — Diagnose Gate** (per `shared/gate-logic.md`):
- Question: "Is this the right diagnosis of the core challenge?"
- TLDR: 3-5 bullets on the diagnosis
- Include: debate highlights, bad strategy flags
- Options: Proceed / Redirect / Kill

### Phase 2: Design

**Goal**: Define the guiding policy and coherent actions.

**Specialist panel** (2 agents — see `roles.md`):
- Strategy Analyst
- Execution Analyst

**Flow**:
1. Team lead proposes a guiding policy grounded in the Phase 1 diagnosis. The policy must explain HOW to address the challenge, not just WHAT outcome is desired.
2. **Parallel analysis**: Strategy Analyst evaluates alternative policies and models competitive responses. Execution Analyst evaluates action feasibility, sequencing, resource requirements, and dependencies.
3. **Debate (1 round)**: Analysts see each other's findings and respond.
4. **Synthesis**: Team lead synthesizes into guiding policy + coherent actions. Each action must have: what, why, dependencies, owner archetype, success signal.
5. **Bad strategy check**: Haiku detector. Key question: "Are these real actions or just goals?"

**Gate 2 — Design Gate** (optional at standard):
Per `shared/gate-logic.md`, Gate 2 auto-proceeds UNLESS:
- Bad strategy detector raised any flags
- Team lead's synthesis includes explicit uncertainty
- Evidence quality is predominantly `[Blog/Opinion]` or `[Unverified]`

If Gate 2 fires:
- Question: "Is this a real strategy, or just goals dressed up?"
- TLDR: guiding policy summary, action list, alignment chain (diagnosis -> policy -> actions)
- Include: debate tensions, bad strategy flags
- Options: Proceed / Redirect / Kill

### Phase 3: Roadmap

**Goal**: Sequence actions into an executable plan.

**Flow**:
1. Team lead sequences the coherent actions from Phase 2 into milestones with timeline signals and success criteria.
2. Build risk register: risk, likelihood, impact, mitigation, owner.
3. Define "Not In Plan": what the strategy explicitly chooses NOT to do, and why.
4. Compile debate summary across all phases.
5. Final bad strategy check on the complete document.

**Gate 3 — Roadmap Gate** (per `shared/gate-logic.md`):
- Question: "Is this roadmap executable? Ready to commit?"
- TLDR: full strategy summary, risk highlights, not-in-plan list
- Include: bad strategy flags
- Options: Proceed / Redirect / Kill

### Output
Save full document to `./pm-workflow/{project-name}/strategy-{project-name}.md` using the template from `templates.md`.

### Cleanup
After Gate 3 resolves (Proceed or Kill), send shutdown requests to all teammates via `SendMessage type: "shutdown_request"`, then run TeamDelete to clean up the agent team.

---

## Deep Mode (--depth deep)

Same as Standard with these changes:
1. **4 skeptics in Phase 1**: Add Data Scientist (see `roles.md`). Focus: data validity, causal claims, methodology rigor.
2. **2 debate rounds**: In every panel, skeptics respond to peers (round 1), then counter-respond to round 1 responses (round 2). Team lead synthesizes the full arc.
3. **Gate 2 required**: No auto-proceed. PM must review the design phase.
4. **10+ web searches per research agent**: Deeper evidence gathering.
5. **DS computational analysis**: Data Scientist runs Python analysis (scipy, numpy, pandas, matplotlib). Check dependencies first per `shared/depth-config.md`.

---

## Rumelt Kernel Enforcement

Read `rumelt.md` at startup. Enforce throughout:

- **Phase 1 MUST produce a diagnosis**: A clear statement of what is actually going on. Not a description of the situation — a judgment about the core challenge.
- **Phase 2 MUST produce a guiding policy**: An approach that explains HOW to overcome the challenge. If it only says WHAT outcome is desired, it fails the kernel test. Phase 2 MUST also produce coherent actions: specific, coordinated steps that implement the policy. If they read like a wish list, they fail.
- **Phase 3 sequences** the actions into a roadmap with milestones and risk mitigation.
- **Bad strategy detector** runs at every gate checking for: fluff, challenge avoidance, goals masquerading as strategy, disconnected objectives.

---

## Formatting Rules

Follow `shared/formatting.md` strictly:
- Bold lead-in for every body paragraph
- Source quality tags on all web data: `[Primary Source]` / `[Industry Report]` / `[Blog/Opinion]` / `[Unverified]`
- Tables for milestones, risk registers, bad strategy checks, objection matrices
- No filler phrases, no hedging without specificity, no buzzwords

---

## Redirect and Kill Behavior

Per `shared/gate-logic.md`:
- **Redirect**: Ask PM for new direction via follow-up question. Team lead re-synthesizes incorporating the redirect. Skeptics do NOT re-run. Re-present the gate. If PM redirects twice on the same gate, suggest Kill + restart.
- **Kill**: Save all partial work to `./pm-workflow/{project-name}/strategy-{project-name}_killed_at_gate_{N}.md`. Run TeamDelete. Report save path.

---

## Task Tracking

Use TaskCreate at the start of each phase to track progress. Update via TaskUpdate as stages complete. Use TaskList to show the PM progress at gates. Example tasks:

```
Phase 1: Diagnose
  [ ] Frame strategic challenge
  [ ] Skeptic initial takes (parallel)
  [ ] Debate round
  [ ] Synthesis
  [ ] Bad strategy check
  [ ] Gate 1

Phase 2: Design
  [ ] Propose guiding policy
  [ ] Analyst parallel review
  [ ] Debate round
  [ ] Synthesis
  [ ] Bad strategy check
  [ ] Gate 2

Phase 3: Roadmap
  [ ] Sequence milestones
  [ ] Risk register
  [ ] Not-in-plan
  [ ] Final bad strategy check
  [ ] Gate 3
```
