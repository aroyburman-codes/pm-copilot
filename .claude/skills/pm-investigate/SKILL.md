# PM Investigate

Turn a PM's hunch into validated evidence with confidence levels, sizing, and an honest assessment of what holds up under scrutiny.

## Invocation

```
/pm-investigate [hypothesis in free text]
/pm-investigate [hypothesis] --depth standard
/pm-investigate [hypothesis] --depth deep --size market --project my-project
```

## Flags

| Flag | Values | Default | Purpose |
|------|--------|---------|---------|
| `--depth` | `quick`, `standard`, `deep` | `quick` | Controls agent count, debate rounds, research depth |
| `--size` | `market`, `opportunity`, `impact`, `effort` | `opportunity` | What to quantify in the sizing section |
| `--context` | file path | none | Feed prior skill output as additional context |
| `--project` | string | auto-generated | Project folder name under `./pm-workflow/` |

## Setup

1. **Parse input**: Extract the hypothesis from free text. Parse flags from the end of the input.
2. **Generate project name**: If `--project` not provided, generate a kebab-case slug from the hypothesis (max 5 words).
3. **Create output folder**: `./pm-workflow/{project-name}/`. If it already exists, warn and ask whether to overwrite or resume.
4. **Auto-detect prior outputs**: Check `./pm-workflow/*/` for any `investigate-*.md`, `strategy-*.md`, or `propose-*.md` files. If found, ask the PM if they want to use any as context.
5. **Load context file**: If `--context` is provided, read that file and include it as background for all agents.
6. **Read supporting files**: At runtime, read these files for detailed role definitions, templates, and shared logic. Resolve paths relative to the skill directory using Glob to find them:
   - `roles.md` (same directory as this SKILL.md)
   - `templates.md` (same directory as this SKILL.md)
   - Find and read `gate-logic.md`, `bad-strategy-detector.md`, `depth-config.md`, `formatting.md` — search for these in the project's `shared/` directory or `.claude/skills/*/` directories

## Depth Routing

Read `shared/depth-config.md` for full behavioral specs. Summary:

- **Quick**: Jump to [Quick Mode Execution](#quick-mode-execution)
- **Standard**: Jump to [Standard/Deep Mode Execution](#standarddeep-mode-execution) with 3 skeptics, 1 debate round
- **Deep**: Jump to [Standard/Deep Mode Execution](#standarddeep-mode-execution) with 4 skeptics, 2 debate rounds

---

## Quick Mode Execution

Single agent. No TeamCreate. Simulate all perspectives internally. Target: 2-3 minutes.

### Step 1: Frame the Hypothesis

Parse the PM's hunch into a testable hypothesis. Structure:
- **Claim**: What specifically is being asserted?
- **Mechanism**: Why would this be true? What's the causal logic?
- **Testable prediction**: What observable evidence would support or refute this?

### Step 2: Multi-Lens Analysis

Run 2-3 WebSearch calls to gather initial evidence. Then analyze through each perspective defined in `roles.md`:
- **Strategic lens**: Market dynamics, competitive landscape, timing
- **Methodologist lens**: Is this testable? What would prove/disprove it?
- **Builder lens**: Technical feasibility, what exists today
- **Data lens**: What base rates exist? What data would we need?

For each lens, state the position and the key tension with other lenses.

### Step 3: Sizing

Per the `--size` flag, produce a back-of-envelope calculation:
- `market`: TAM/SAM/SOM estimates with methodology
- `opportunity`: Addressable market + realistic capture estimate
- `impact`: User or business impact quantification
- `effort`: Engineering/resource effort estimate

State all assumptions explicitly. Tag sources.

### Step 4: Bad Strategy Self-Check

Run the four-hallmark check from `shared/bad-strategy-detector.md` on your own output:
- Fluff, Challenge Avoidance, Goals-as-Strategy, Bad Objectives

### Step 5: Single Gate

Present findings using the gate format from `shared/gate-logic.md`.

Use AskUserQuestion:
- **Question**: "Do you trust this finding enough to act on it?"
- **Options**: Proceed / Redirect / Kill
- Show: TLDR (3-5 bullets), key findings, sizing summary, bad strategy flags

**On Proceed**: Save output using the template from `templates.md` to `./pm-workflow/{project-name}/investigate-{project-name}.md`.

**On Redirect**: Ask for new direction, re-synthesize (don't start over), re-present gate.

**On Kill**: Save partial work with `_killed` suffix. Report save location.

---

## Standard/Deep Mode Execution

Uses agent teams with parallel skeptics and structured debate.

### Dependency Check

Before starting, verify Python packages are available (needed for Phase 2):

```bash
python3 -c "import numpy, pandas, matplotlib, scipy" 2>&1
```

If any import fails, install: `pip install numpy pandas matplotlib scipy`. Log what was installed.

### Phase 1: Explore

**Goal**: Pressure-test whether this is the right question to investigate.

#### 1a. Team Lead Frames Hypothesis

Parse the hunch into a testable hypothesis (same structure as Quick Step 1). Share with the team.

#### 1b. Create Skeptic Team

Use TeamCreate. Read `roles.md` for full role definitions.

**Standard** (3 skeptics):
- Team Lead (orchestration, synthesis)
- Strategic Skeptic (market dynamics, timing, competitive landscape)
- Methodologist (testability, evidence design, null hypothesis)
- Builder (technical feasibility, capability gaps, prior art)

**Deep** (4 skeptics — add Data Scientist):
- All of the above, plus:
- Data Scientist (base rates, statistical power, data availability)

#### 1c. Initial Takes (Parallel)

Send the framed hypothesis to all skeptics simultaneously. Each skeptic:
- Analyzes through their lens (see `roles.md` for detailed prompts)
- Runs web searches (5-8 per agent at standard, 10+ at deep)
- Produces a written position with evidence and source quality tags
- Tags all web-sourced data: `[Primary Source]` / `[Industry Report]` / `[Blog/Opinion]` / `[Unverified]`

#### 1d. Debate

**Stage: Cross-Pollination**. Team lead compiles ALL initial takes and sends the full compilation to ALL skeptics via SendMessage.

Each skeptic reads all peer arguments and responds:
- Challenge specific claims from other skeptics
- Strengthen or weaken their own position based on new information
- Flag contradictions across perspectives

**Standard**: 1 debate round (respond once).
**Deep**: 2 debate rounds (respond, then counter-respond after seeing Round 1 responses).

#### 1e. Synthesis

Team lead synthesizes the full debate into findings:
- **Agreements**: Where skeptics converged
- **Unresolved tensions**: Where they disagreed and why
- **Confidence-weighted conclusions**: What we believe and how strongly

#### 1f. Bad Strategy Check

Spawn a separate Haiku agent (or use Agent tool) with the prompt from `shared/bad-strategy-detector.md`. Feed it the synthesis. Collect flags.

#### 1g. Gate 1 — Explore Gate

Present via AskUserQuestion per `shared/gate-logic.md`:
- **Question**: "Is this hypothesis sharp enough to investigate?"
- **TLDR**: 3-5 bullets on whether this is the right question
- **Show**: Reframed hypothesis, debate highlights, bad strategy flags, suggested refinements
- **Options**: Proceed / Redirect / Kill

**Redirect handling**: Team lead re-synthesizes with PM's new direction. Skeptics do NOT re-run. Re-present gate. If PM redirects twice on the same gate, suggest Kill + restart.

**Kill handling**: Save partial work, add `_killed_at_gate_1.md` suffix, clean up team.

---

### Phase 2: Analyze

**Goal**: Run the actual investigation with proper methodology.

#### 2a. Reconfigure Team

After Phase 1, the team shifts to research mode. The active agents are now:
- **Team Lead** (Opus): Orchestration, quality control
- **Researcher** (Sonnet): Deep web research — market data, reports, case studies, expert opinions
- **Data Scientist** (Sonnet): Methodology design + computational analysis

Use SendMessage to assign new directives, or spawn new Agent instances for the Researcher and DS roles.

#### 2b. Research (Parallel)

**Researcher** runs deep web research based on the refined hypothesis from Phase 1:
- 5-8 searches at standard, 10+ at deep
- Collects: market data, analyst reports, case studies, expert opinions, counter-examples
- Tags every data point with source quality: `[Primary Source]` / `[Industry Report]` / `[Blog/Opinion]` / `[Unverified]`

**Data Scientist** designs methodology AND runs computational analysis:
- Designs the measurement approach: what to measure, how, confidence framework
- Writes and executes Python scripts via Bash for:
  - Sizing calculations (per `--size` flag)
  - Statistical analysis (scipy, numpy)
  - Monte Carlo simulations for outcome ranges (deep mode)
  - Sensitivity analysis on key assumptions
  - Visualizations saved as PNGs to `./pm-workflow/{project-name}/charts/`
- Explicitly states all assumptions
- Searches for counter-evidence that would invalidate the hypothesis

#### 2c. Team Lead Quality Check

Team lead reviews both outputs for:
- Alignment between research findings and methodology
- Integration of Phase 1 skeptic findings
- Source quality distribution (flag if mostly `[Blog/Opinion]` or `[Unverified]`)
- Gaps between what was hypothesized and what evidence supports

#### 2d. Bad Strategy Check

Run the Haiku bad strategy detector on the combined Phase 2 output.

#### 2e. Gate 2 — Analyze Gate

**Standard depth**: Gate 2 is OPTIONAL. Auto-proceed UNLESS any of these are true:
- Bad strategy detector raised any flags
- Team lead's synthesis includes explicit uncertainty or concern
- Evidence quality is predominantly `[Blog/Opinion]` or `[Unverified]`

If auto-proceeding, log: "Gate 2 auto-approved (no concerns flagged)" and move to Phase 3.

**Deep depth**: Gate 2 is REQUIRED.

When Gate 2 fires:
- **Question**: "Is this methodology sound? Are we measuring the right things?"
- **TLDR**: Evidence collected (with source tags), methodology, DS analysis outputs, data gaps
- **Options**: Proceed / Redirect / Kill

---

### Phase 3: Synthesize

**Goal**: Produce the final findings document.

#### 3a. Document Assembly

Team lead produces the final document incorporating all prior work. Use the full template from `templates.md`. Apply formatting rules from `shared/formatting.md`:
- TLDR first (always)
- Bold lead-in on every body paragraph
- Source quality tags on all web-sourced data
- Tables for structured data
- Chart references for DS visualizations

#### 3b. Bad Strategy Check

Final Haiku detector pass on the complete document.

#### 3c. Gate 3 — Synthesize Gate

- **Question**: "Do you trust this finding enough to act on it?"
- **TLDR**: Final findings summary, confidence level, biggest risk, bad strategy flags
- **Options**: Proceed / Redirect / Kill

**On Proceed**: Save to `./pm-workflow/{project-name}/investigate-{project-name}.md`. Clean up team. Report file location.

**On Redirect**: Team lead re-synthesizes with PM's direction. Do not re-run skeptics or research. Re-present gate.

**On Kill**: Save partial work with `_killed_at_gate_3.md` suffix, clean up team, report location.

---

## Task Tracking

Use TaskCreate at the start to create a checklist the PM can see. Update via TaskUpdate as phases complete.

**Quick mode tasks**:
1. Frame hypothesis
2. Multi-lens analysis + research
3. Sizing
4. Bad strategy check + gate

**Standard/Deep mode tasks**:
1. Frame hypothesis
2. Skeptic panel — initial takes
3. Skeptic panel — debate
4. Synthesis + Gate 1
5. Research + DS analysis
6. Quality check + Gate 2
7. Final document + Gate 3

---

## Error Handling

- If a skeptic agent fails or times out, proceed with the remaining agents. Note the missing perspective in the synthesis.
- If Python package installation fails, fall back to LLM-only reasoning for DS analysis. Note the limitation.
- If web search returns no results for a query, try 2 alternative phrasings before marking as a data gap.
- If the PM redirects twice on the same gate, suggest Kill + restart with a refined hypothesis.

## Output Location

All outputs save to `./pm-workflow/{project-name}/`:
- `investigate-{project-name}.md` — final findings document
- `charts/` — any DS visualizations (standard+ only)
- `_killed_at_gate_{N}.md` — partial work if killed
