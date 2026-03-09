# Depth Configuration Reference

## Parsing the --depth Flag

Extract from the user's input. Default to `quick` if not specified.

```
Valid values: quick | standard | deep
```

## Behavior by Depth

### Quick (default)
| Aspect | Behavior |
|--------|----------|
| Agent pattern | Single Opus agent simulates all skeptic perspectives |
| Team creation | No TeamCreate — runs in main session |
| Debate | None — perspectives explored in one prompt |
| Web research | 2-3 WebSearch calls total |
| DS analysis | LLM reasoning only, no Python scripts |
| Gates | 1 combined gate at the end of all phases |
| Output | Condensed single document |
| Target time | 2-3 minutes |

**Quick mode prompt pattern**: The single agent should structure its thinking as:
1. Frame the input
2. Consider it through each skeptic lens (name each perspective)
3. Note tensions between perspectives
4. Synthesize findings
5. Run bad strategy self-check
6. Present single gate to PM

### Standard
| Aspect | Behavior |
|--------|----------|
| Agent pattern | 3 parallel Sonnet skeptics + Opus team lead |
| Team creation | TeamCreate with team lead + 3 skeptics + specialists |
| Debate | 1 round — all initial takes shared, each skeptic responds once |
| Web research | 5-8 WebSearch calls per research agent |
| DS analysis | Python scripts via Bash (sizing, statistics, charts) |
| Gates | 3 gates, Gate 2 optional (auto-proceeds unless concerns flagged) |
| Output | Full document with all sections |
| Target time | 8-12 minutes |

**Standard debate flow**:
1. Spawn 3 skeptics with `run_in_background: true`
2. Collect all outputs
3. Send synthesis of ALL takes to each skeptic via SendMessage
4. Collect debate responses
5. Team lead produces final synthesis

### Deep
| Aspect | Behavior |
|--------|----------|
| Agent pattern | 4 parallel Sonnet skeptics + Opus team lead |
| Team creation | TeamCreate with team lead + 4 skeptics + specialists |
| Debate | 2 rounds — respond, then counter-respond |
| Web research | 10+ WebSearch calls per research agent |
| DS analysis | Full quantitative: Monte Carlo, sensitivity, multiple models |
| Gates | 3 gates, all required |
| Output | Comprehensive document with appendices |
| Target time | 15-25 minutes |

**Deep debate flow**:
1. Spawn 4 skeptics with `run_in_background: true`
2. Collect all outputs
3. Round 1: Share all takes, each skeptic responds
4. Collect Round 1 responses
5. Round 2: Share Round 1 responses, each skeptic counter-responds
6. Collect Round 2 responses
7. Team lead produces final synthesis incorporating full debate arc

## Dependency Check (Standard+ only)

Before starting computational analysis, check for required Python packages:

```bash
python3 -c "import numpy, pandas, matplotlib, scipy" 2>&1
```

If any import fails, install:
```bash
pip install numpy pandas matplotlib scipy
```

Log which packages were installed so the PM knows their environment was modified.
