# Good Strategy Kernel — Rumelt Reference

Richard Rumelt's "Good Strategy Bad Strategy" defines the kernel of good strategy as three elements that work together: a diagnosis, a guiding policy, and coherent actions. This file is the reference for the pm-strategize skill. Read it before starting any strategy work.

---

## The Kernel

### 1. Diagnosis

**What it is**: A judgment about the nature of the challenge. The diagnosis simplifies the overwhelming complexity of reality by identifying the critical aspects of the situation.

**What it is NOT**:
- A description of the situation (that is a summary, not a diagnosis)
- A list of problems (that is an inventory, not a judgment)
- A restatement of the goal (that is circular)

**How to test a diagnosis**:
- Does it name a specific obstacle or challenge?
- Does it explain WHY this is the core challenge and not something else?
- Does it simplify — does reading it make the path forward clearer?
- Could a thoughtful outsider disagree with this diagnosis? (If no one could disagree, it is probably too obvious to be useful.)

**Example — Strong diagnosis**:
> "Our core challenge is that our product serves two audiences with conflicting needs. Enterprise buyers want stability and predictability; individual users want rapid iteration and experimentation. Trying to serve both with one product line is making us mediocre at both."

**Example — Weak diagnosis**:
> "We need to grow faster in a competitive market." (This is a goal restated as a situation. It does not identify what is actually making growth difficult.)

---

### 2. Guiding Policy

**What it is**: The overall approach chosen to cope with or overcome the challenge identified in the diagnosis. It channels and constrains action in a specific direction.

**What it is NOT**:
- A goal or vision ("Become the market leader")
- A list of priorities ("Focus on growth, retention, and quality")
- A vague aspiration ("Deliver world-class experiences")

**The HOW test**: A guiding policy must explain the mechanism. Ask: "How does this address the diagnosed challenge?" If the answer is "by achieving the desired outcome," it fails — that is circular.

**How to test a guiding policy**:
- Does it create advantage by applying strength against weakness?
- Does it rule things out? (A policy that permits everything directs nothing.)
- Does it follow logically from the diagnosis?
- Can you explain WHY this approach works against the specific challenge diagnosed?

**Example — Strong guiding policy**:
> "We will split into two product lines with separate release cadences — an enterprise tier on quarterly releases with LTS guarantees, and a pro tier on continuous deployment. Shared infrastructure underneath, divergent surfaces above."

**Example — Weak guiding policy**:
> "We will focus on delighting both enterprise and individual users through excellence." (This is a restatement of the goal. It does not explain HOW the conflicting-needs problem gets resolved.)

---

### 3. Coherent Actions

**What they are**: A coordinated set of specific actions that implement the guiding policy. "Coherent" means the actions work together — they reinforce each other rather than pulling in different directions.

**What they are NOT**:
- A wish list of good things to do
- A set of independent initiatives that happen to be running in parallel
- Goals or metrics ("Increase retention by 20%")

**How to test coherent actions**:
- Is each action specific enough that someone could start executing it?
- Do the actions reinforce each other? (Pull one out — does it weaken the others?)
- Are they sequenced? (What depends on what?)
- Do they implement the guiding policy? (Could you swap in a different policy and these actions would still make sense? If yes, they are not coherent with this policy.)
- Do they concentrate effort? (A long list of equal-priority actions is a sign of no real strategy.)

**Example — Strong coherent actions**:
> 1. Fork the codebase into enterprise/pro branches by end of Q1 (dependency: none — this is the gate)
> 2. Ship enterprise LTS v1 with migration tooling by Q2 (dependency: fork complete)
> 3. Accelerate pro tier to weekly releases, starting with the 3 most-requested features from the user community (dependency: fork complete)
> 4. Assign dedicated support team to enterprise tier; redirect pro support to community-first model (dependency: LTS v1 shipped)

**Example — Weak coherent actions**:
> 1. Improve product quality
> 2. Increase customer satisfaction
> 3. Expand market share
> 4. Hire more engineers
>
> (These are goals, not actions. They are not sequenced. They do not implement a specific policy.)

---

## The Coherence Test

The three elements must chain together:

```
DIAGNOSIS: [What is the core challenge?]
     ↓ (therefore)
GUIDING POLICY: [What approach addresses this challenge?]
     ↓ (implemented by)
COHERENT ACTIONS: [What specific, coordinated steps execute the policy?]
```

Test the chain by reading it as a narrative:
> "Because [diagnosis], we will [guiding policy], which means specifically doing [actions]."

If any link is weak — the policy does not follow from the diagnosis, or the actions do not implement the policy — the strategy has a structural gap.

---

## Four Hallmarks of Bad Strategy

These are the patterns to detect and flag. The bad strategy detector checks for all four at every gate.

### 1. Fluff

Superficial restatement of the obvious combined with buzzwords. Fluff creates the illusion of thinking.

**Pattern**: Replace the jargon with plain language. If the sentence then says nothing new, it is fluff.

**Example**: "Our strategy is to leverage our synergistic capabilities to transform the customer experience paradigm." (Translation: we will use what we have to do better. This says nothing.)

### 2. Failure to Face the Challenge

The strategy avoids naming the hard part. It dances around the central difficulty rather than confronting it.

**Pattern**: Read the diagnosis and ask: "What is the single hardest thing about this situation?" If the strategy does not name it and address it, this hallmark is present.

**Example**: A retail company facing online disruption produces a strategy about "operational efficiency" and "customer loyalty programs" without ever naming the structural shift to e-commerce. The hard part — that their physical stores are becoming a liability — is never addressed.

### 3. Mistaking Goals for Strategy

Statements of desired outcomes presented as if they were a strategy. Performance goals, budgets, or ambitions are not strategy.

**Pattern**: For each statement, ask: "How?" If the answer is just another goal, this hallmark is present.

**Example**: "Our strategy is to grow revenue by 25% while maintaining margins." How? "By focusing on high-value customers." How do you focus on them? "By providing superior value." (This is a stack of goals all the way down, with no mechanism.)

### 4. Bad Strategic Objectives

Objectives that fail to address the diagnosis, or that form a sprawling, disconnected list.

**Two forms**:
- **Dog's dinner objectives**: A long list of things to do that lacks an overall structure. Typically the result of political negotiation rather than strategic thinking — every team's pet project gets listed.
- **Blue sky objectives**: Objectives so ambitious they are detached from reality, with no consideration of how to get there.

**Pattern**: Check whether each objective connects back to the diagnosis. If an objective would exist regardless of the diagnosis, it is probably not strategic.

---

## Application in pm-strategize

### Phase 1 (Diagnose)
Produce a diagnosis that passes these tests:
- Names a specific core challenge
- Explains why THIS is the core challenge
- Simplifies the situation — makes the path forward clearer
- Is debatable (not a tautology)

### Phase 2 (Design)
Produce a guiding policy and coherent actions that pass these tests:
- Policy explains HOW, not WHAT
- Policy follows logically from the diagnosis
- Policy rules things out
- Actions are specific and executable
- Actions reinforce each other (coherence)
- Actions implement the policy (not a generic to-do list)
- Actions are sequenced with dependencies

### Phase 3 (Roadmap)
Sequence actions into milestones. Add:
- Risk register (what threatens the strategy)
- Not-in-plan list (what the strategy deliberately excludes)

### At Every Gate
Run the four-hallmark check. Flag anything that fails. Do not proceed with unaddressed flags.

---

## Common Pitfalls

1. **Diagnosis by analogy**: "We are the Uber of X." Analogy is not diagnosis. It borrows someone else's analysis instead of doing your own.

2. **Policy by consensus**: A guiding policy that tries to please everyone ("and" instead of "or") signals no real choice was made. Good policy requires trade-offs.

3. **Action by delegation**: "Engineering will figure out the technical approach" is not an action. It is a hope. Coherent actions specify what gets done, not who you hope will figure it out.

4. **Mistaking proximate for root cause**: The diagnosis identifies symptoms rather than the underlying dynamic. Ask "why?" at least twice. If the diagnosis would change with one more "why?", it has not gone deep enough.

5. **Strategy as budget allocation**: Distributing resources across all possible options is the opposite of strategy. Strategy concentrates resources on the decisive point.

6. **Confusing operational effectiveness with strategy**: Doing what you already do but better is not a strategy. Strategy involves making different choices, not just executing existing choices more efficiently.
