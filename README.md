# PM Copilot

**Agent-team powered PM workflows for Claude Code.**

PM Copilot runs parallel AI agent teams — each with different analytical lenses — that debate each other before synthesizing findings. You drive every decision through approval gates. The result: higher-quality investigation, strategy, and proposals with less blind-spot risk.

```
/pm-investigate I think vertical SaaS for construction will consolidate in 18 months
/pm-strategize Build a platform play around our API before competitors catch up
/pm-propose Pitch the data infrastructure investment to the CTO --mode decision
```

---

## Why PM Copilot

Most AI PM tools are single-prompt wrappers. You paste a question, get a monologue back. PM Copilot is different in three ways:

**Multi-agent debate, not monologue.** Each skill spawns 3-4 specialized agents that analyze your problem from different angles, then challenge each other's arguments before synthesis. A strategic skeptic and a builder skeptic will reach different conclusions — the tension between them is where insight lives.

**You drive the decisions.** Nothing advances without your approval at phase gates. You see a TLDR, the key tensions from the debate, and any quality flags. Then you decide: proceed, redirect, or kill. The agents execute; you steer.

**A bad strategy detector watches everything.** An independent quality checker runs at every gate, flagging the four hallmarks of bad strategy: fluff, challenge avoidance, mistaking goals for strategy, and disconnected objectives. It catches weak thinking before it reaches your stakeholders.

---

## How It Works

### Agent Teams with Skeptic Panels

```
Your input (hypothesis / challenge / pitch)
     │
     ▼
┌──────────────────────────────────────────┐
│  PARALLEL ANALYSIS                       │
│  3-4 skeptics with different lenses      │
│  (strategy, feasibility, data, cost...)  │
└──────────┬───────────────────────────────┘
           │ all positions shared
           ▼
┌──────────────────────────────────────────┐
│  CROSS-POLLINATION DEBATE                │
│  Each skeptic reads peer arguments and   │
│  challenges, amends, or strengthens      │
└──────────┬───────────────────────────────┘
           │
           ▼
┌──────────────────────────────────────────┐
│  SYNTHESIS + QUALITY CHECK               │
│  Team lead synthesizes debate            │
│  Independent bad-strategy detector runs  │
└──────────┬───────────────────────────────┘
           │
           ▼
┌──────────────────────────────────────────┐
│  YOU DECIDE                              │
│  Proceed / Redirect / Kill               │
│  Nothing advances without your approval  │
└──────────────────────────────────────────┘
```

### Three Skills, One Workflow

| Skill | Purpose | Output |
|-------|---------|--------|
| `/pm-investigate` | Turn a hunch into validated evidence | Findings doc with hypothesis, evidence, confidence levels, sizing |
| `/pm-strategize` | Build a strategy and roadmap | Strategy doc with diagnosis, guiding policy, coherent actions, milestones |
| `/pm-propose` | Pitch to a decision maker | Proposal doc (formal decision ask or steering discussion) |

Each skill works standalone. When used in sequence, later skills auto-detect earlier outputs and offer to build on them.

---

## Installation

```bash
# Clone the repo
git clone https://github.com/aroyburman-codes/pm-copilot.git

# Copy skills and shared files to your project
cp -r pm-copilot/.claude your-project/
cp -r pm-copilot/shared your-project/shared
```

Or install individual skills:

```bash
# Copy one skill + shared references
cp -r pm-copilot/.claude/skills/pm-investigate your-project/.claude/skills/
cp -r pm-copilot/shared your-project/shared
```

The `shared/` directory goes in your project root. Skills reference it at runtime.

Requires [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI.

---

## Usage

### PM Investigate

Turn a hypothesis into evidence.

```bash
# Quick (default) — single agent, 2-3 min
/pm-investigate I think AI coding tools will 10x developer productivity by 2027

# Standard — agent team with debate, 8-12 min
/pm-investigate I think AI coding tools will 10x developer productivity --depth standard

# Deep — full panel, 2 debate rounds, quantitative analysis, 15-25 min
/pm-investigate I think vertical SaaS consolidation is accelerating --depth deep

# With sizing type
/pm-investigate The market for AI legal tools is underserved --size market
```

**What you get**: Findings document with refined hypothesis, evidence (supporting + counter), confidence assessment, quantitative sizing, objection response matrix, and a clear recommendation.

**At standard+ depth**: The data scientist agent runs actual Python analysis — statistical tests, Monte Carlo simulations, market sizing calculations with explicit assumptions. Not just LLM reasoning.

### PM Strategize

Build a strategy using the Good Strategy Kernel (diagnosis → guiding policy → coherent actions).

```bash
# Quick
/pm-strategize We need to respond to competitors shipping AI features faster than us

# Standard with prior investigation as context
/pm-strategize Build a platform strategy for our API --depth standard

# Deep
/pm-strategize Enter the AI developer tools market --depth deep
```

**What you get**: Strategy document with diagnosis (the actual challenge), guiding policy (how to win), coherent actions (what to do), milestones, risk register, and explicit "not in plan" section.

**Built-in quality check**: A bad strategy detector runs at every gate, flagging fluff, goal-as-strategy, challenge avoidance, and disconnected objectives. Based on Richard Rumelt's hallmarks of bad strategy.

### PM Propose

Pitch a strategy to a specific decision maker.

```bash
# Decision mode (default) — formal proposal with a clear ask
/pm-propose Pitch the data platform investment to the VP of Engineering --mode decision

# Steer mode — discussion doc asking for directional input
/pm-propose Get the CTO's read on our AI integration approach --mode steer

# Standard depth
/pm-propose Propose expanding into enterprise --depth standard --mode decision
```

**Decision mode output**: Executive summary, the case, objection response matrix, the specific ask, alternatives considered, risk mitigation.

**Steer mode output**: Current thinking, open questions with your lean on each, trade-offs table, what specific input you need from them.

---

## Depth Levels

| Depth | Agents | Debate | Analysis | Gates | Time | Est. Cost |
|-------|--------|--------|----------|-------|------|-----------|
| `quick` (default) | 1 (simulates all perspectives) | None | LLM reasoning | 1 | 2-3 min | ~$0.50 |
| `standard` | 3 skeptics + lead | 1 round | Python (stats, charts) | 3* | 8-12 min | ~$3-5 |
| `deep` | 4 skeptics + lead | 2 rounds | Full quantitative | 3 | 15-25 min | ~$8-15 |

*Gate 2 auto-approves at standard depth unless concerns are flagged.

Cost estimates are approximate and depend on hypothesis complexity and research depth.

---

## Skeptic Panels

Each skill has role-specific skeptics that pressure-test from different angles:

### Investigate
| Role | Lens |
|------|------|
| Strategic | Market dynamics, competitive landscape, timing |
| Methodologist | Testability, evidence requirements, null hypothesis |
| Builder | Technical feasibility, existing solutions, capability gaps |
| Data Scientist* | Data availability, base rates, statistical rigor |

### Strategize
| Role | Lens |
|------|------|
| Investor | Investability, disruption potential, market prediction |
| PM | Opportunity structure, credibility, fatal flaws |
| Builder | Technical feasibility, infrastructure gaps, build vs buy |
| Data Scientist* | Data validity, causal claims, methodology |

### Propose
| Role | Lens |
|------|------|
| Approver | Decision-maker's perspective, what they care about |
| Alternatives | Competing priorities, opportunity cost |
| Feasibility | Deliverability, dependencies, what could go wrong |
| True Cost* | Direct, opportunity, political capital, and failure costs |

*4th skeptic activates at deep depth only.

---

## Bad Strategy Detector

An independent quality checker (runs on a separate, lightweight model) evaluates every phase output against four hallmarks of bad strategy:

| Hallmark | What It Catches |
|----------|----------------|
| **Fluff** | Buzzwords masquerading as insight. If you replace jargon with plain language and the sentence says nothing, it gets flagged. |
| **Challenge avoidance** | Sidestepping the hard part. The analysis talks around the difficulty rather than confronting it. |
| **Goals as strategy** | Stating desired outcomes without explaining the mechanism. "Grow revenue 25%" is a goal, not a strategy. |
| **Bad objectives** | Disconnected wish lists, objectives that don't trace back to the core challenge. |

If any flag is raised, Gate 2 (normally optional at standard depth) requires manual review. You always see the flags before deciding.

---

## Gates

Every phase ends with a decision point where you see:
- **TLDR**: 3-5 bullets summarizing findings
- **Key tensions**: What the skeptic debate surfaced
- **Bad strategy flags**: Any quality issues detected
- **Your decision**: Proceed / Redirect / Kill

**Redirect** adjusts direction without re-running the full analysis. **Kill** saves all work done so far and stops.

---

## Output Structure

All outputs go to `./pm-workflow/{project-name}/`:

```
pm-workflow/
└── vertical-saas-consolidation/
    ├── investigate-vertical-saas-consolidation.md
    ├── strategy-vertical-saas-consolidation.md
    ├── propose-vertical-saas-consolidation.md
    └── charts/
        ├── market-sizing.png
        └── sensitivity-analysis.png
```

---

## How Skills Connect

Skills are standalone — use any one independently. When used in sequence, they auto-detect prior work:

```
/pm-investigate AI coding tools market opportunity --depth standard
  → Produces: ./pm-workflow/ai-coding/investigate-ai-coding.md

/pm-strategize Enter the AI coding tools market
  → Detects investigate output, asks: "Use prior investigation as context?"
  → Produces: ./pm-workflow/ai-coding/strategy-ai-coding.md

/pm-propose Pitch to VP Engineering --mode decision
  → Detects strategy output, asks: "Use prior strategy as context?"
  → Produces: ./pm-workflow/ai-coding/propose-ai-coding.md
```

You can also pass context explicitly: `--context ./path/to/file.md`

---

## Example Output (Quick Mode)

Running `/pm-investigate I think vertical SaaS for construction will consolidate in 18 months` produces output like:

```markdown
# Investigation: Vertical SaaS Construction Consolidation

## TLDR
- Construction SaaS is mid-consolidation, not pre-consolidation — 3 major acquisitions
  in the last 12 months signal the wave is already underway
- 18-month timeline is aggressive; historical vertical SaaS consolidation cycles run
  24-36 months from first major acquisition to market stabilization
- Medium confidence — strong acquisition data but limited public financial disclosure
  from key players makes sizing uncertain
- Opportunity sizing: $4.2B addressable market with ~40% likely to consolidate
- Recommended next step: validate with standard-depth investigation focusing on
  acquirer financial capacity and remaining independent targets

## Hypothesis
**Original**: Vertical SaaS for construction will consolidate in 18 months
**Refined**: The construction SaaS market (project management, estimating, field
operations) will see 50%+ of current independent vendors acquired or merged within
18 months, driven by PE roll-up strategies and platform plays from incumbents.

## Analysis

### Strategic Lens
**Three PE-backed roll-ups are already active in this space**: Thoma Bravo's
acquisition of Procore competitors, Vista Equity's construction portfolio
consolidation, and Trimble's vertical integration strategy suggest the thesis is
directionally correct but the timeline needs adjustment...

### Methodological Lens
**The 18-month claim is testable but the definition of "consolidation" needs
tightening**: Does consolidation mean 50% of vendors acquired? Revenue concentration
above HHI threshold? We need a specific, measurable definition before this hypothesis
can be validated or falsified...

[... continues with Builder Lens, Data Lens, Tensions, Sizing, Confidence,
Bad Strategy Check, and Recommendation]
```

---

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- Python 3.8+ (for DS computational analysis at standard/deep depth)
- At standard+ depth, the skill auto-installs numpy, pandas, matplotlib, scipy if not present

---

## Contributing

Pull requests welcome. If you build a new skeptic role, gate pattern, or output template, open a PR.

---

## License

MIT
