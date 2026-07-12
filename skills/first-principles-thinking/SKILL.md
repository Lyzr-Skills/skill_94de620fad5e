---
name: first-principles-thinking
description: >
  Apply first principles thinking to engineering problems. Use when facing
  architecture decisions, system design, technology selection, complex debugging,
  or any problem where inherited assumptions might lead to suboptimal solutions.
  Triggers on "first principles", "FP mode", "think from scratch", "why are we
  doing it this way", or auto-detects complex decision-making contexts like
  architecture, scaling, migration, and hard debugging. Use this skill even
  when the user doesn't explicitly mention first principles -- any time they're
  about to make a significant technical decision based on convention rather than
  analysis, or when phrases like "best practice", "everyone does it this way",
  or "industry standard" appear, this skill should activate.
---

# First Principles Thinking

Break problems down to their fundamental truths and rebuild solutions from the ground up.
Instead of reasoning by analogy ("how did others solve this?"), reason from bedrock
("what is actually true here, and what can we build from that?").

> "The first basis from which a thing is known." -- Aristotle

<HARD-GATE>
Do NOT jump to implementation, recommend a technology, or propose a solution until:
1. The problem has been restated and confirmed
2. Assumptions have been surfaced and challenged
3. Ground truths have been separated from inherited conventions

Why this matters: premature solutions lock in assumptions before they've been examined.
The most expensive engineering mistakes come from solving the wrong problem efficiently.
</HARD-GATE>

---

## When This Skill Activates

### Auto-Trigger Signals

The skill activates when the request contains complexity signals:

- **Architecture/design:** "design", "architect", "how should I structure", "what pattern"
- **Technology selection:** "should I use X or Y", "what database", "which framework"
- **Complex debugging:** "keeps failing", "can't figure out why", "intermittent", "root cause"
- **Performance/scaling:** "optimize", "scale", "bottleneck", "slow"
- **Integration/migration:** "connect X to Y", "migrate from", "replace X with"
- **Explicit invocation:** "first principles", "FP mode", "think from scratch"

### Skip Signals

Stay dormant when the request is clearly scoped and simple:

- **Direct implementation:** "add a button", "write a function that", "fix this typo"
- **Small changes:** "rename X to Y", "add validation for", "update the config"
- **Boilerplate:** "create a new component", "scaffold", "set up the project"
- **User override:** "just do it", "skip the analysis", "no need to think deeply"

---

## Depth Levels

Determine depth based on problem complexity. State the detected level so the user can override.

| Level | When | What Runs | Interaction Time |
|----------|--------------------------------------|-------------------------------------------|------------------|
| Quick | Medium-complexity or manual `/fp` | Intake + 2-3 targeted questions + compact artifact | ~1-2 min |
| Standard | Architecture, design, tech selection | Intake + iterative questioning + decomposition map + full artifact | ~3-5 min |
| Deep | System design, hard debugging, `/fp deep` | All phases including multiple reconstruction paths | ~5-10 min |

---

## The Phases

### Phase 1: Problem Intake

Restate the problem in your own words. Ask:

> "Here's what I understand you're trying to solve: [restated problem]. Is that right?"

Then state the detected depth level:

> "This looks like a [Quick/Standard/Deep]-depth problem. Want me to adjust?"

Wait for the user to confirm before proceeding. Getting the problem statement
wrong wastes the entire analysis.

### Phase 2: Socratic Questioning

Challenge the problem using six question types adapted for engineering.
Pick the most relevant types for the problem at hand -- don't robotically
ask all six.

**1. Clarification**
- "What exactly do you mean by X?"
- "Can you give me a concrete example of the problem?"
- "What does success look like here?"

**2. Assumption Probing**
- "Why does it need to work that way?"
- "What if that constraint didn't exist?"
- "Is that a hard requirement or something you inherited from the current design?"
- "Who decided it has to be done this way, and what was their reasoning?"

**3. Evidence**
- "What data supports that this is the bottleneck?"
- "How do you know users actually need this?"
- "Have you measured this, or is it a guess?"

**4. Alternative Viewpoints**
- "What would someone who disagrees with this approach say?"
- "How would a team with completely different constraints solve this?"
- "What would you build if you were starting from zero today?"

**5. Implications**
- "If we go this route, what are the second-order effects?"
- "What breaks if this assumption turns out to be wrong?"
- "What's the cost of reversing this decision later?"

**6. Meta**
- "Are we solving the right problem?"
- "Is this the simplest version of this problem?"
- "What would happen if we just... didn't do this?"

**Red flags for hidden assumptions** -- these phrases almost always signal
an assumption disguised as a constraint:
- "We've always done it this way"
- "Industry standard says..."
- "Everyone uses X for this"
- "That's too simple to work"
- "We can't change that"
- "The client/PM said so" (without verifying the underlying need)

When you hear these, dig deeper with assumption-probing questions.

**Questioning cadence by depth:**
- **Quick:** Pick 2-3 most impactful questions. Ask them together in one message.
- **Standard:** Ask 1-2 questions at a time. Adapt follow-ups based on answers.
- **Deep:** Ask 1 question at a time. Go wherever the reasoning leads. Follow threads.

### Phase 3: Decomposition (Standard + Deep)

Break the problem into atomic components. Tag each one:

- **Ground Truth** -- Constrained by physics, math, verified business rules, or
  hard technical requirements. These cannot be changed. Mark with `[TRUTH]`.
- **Assumption** -- Inherited convention, habit, team preference, or unverified
  belief. These can be challenged or removed. Mark with `[ASSUMPTION]`.

**Ground Truth Test** -- before tagging something as `[TRUTH]`, verify:
1. Can this be further decomposed into something more fundamental?
2. Is this provably true, not just commonly believed?
3. Would violating this definitely cause failure?

If the answer to any question is "no" or "not sure", it's likely an assumption.

**Assumption categories** -- classify each assumption to ensure full coverage:

| Category | Key Question |
|----------|-------------|
| Technical | "Must we use this technology or pattern?" |
| Business | "Is this requirement actually fixed, or negotiable?" |
| Resource | "Are these constraints real or just perceived?" |
| Historical | "Why was this decision made originally? Do those conditions still hold?" |

**Adaptive behavior:**
- If you are confident in the domain, propose the full decomposition and ask the
  user to review, correct, or confirm each tag.
- If the domain is unfamiliar, switch to collaborative mode: ask the user targeted
  questions to co-discover which components are ground truths vs assumptions.

**Recursion rule:** When decomposition reveals a sub-problem with its own hidden
assumptions (e.g., "we need a message queue" leads to "do we actually need async
processing?"), flag it:

> "This sub-problem has its own assumptions worth examining. Going one level deeper."

Run Phases 2-3 on the sub-problem, then return to the parent decomposition.
Maximum recursion depth: 2 levels. If you hit the limit, note the unexplored
sub-problem in the artifact for future analysis.

### Phase 4: Reconstruction (Deep only, optional at Standard)

Build 2-3 solution paths exclusively from verified ground truths. For each path:

- State which ground truths it builds on
- Call out every design choice (where you could have gone differently)
- State trade-offs vs the other paths
- Be honest about unknowns and risks

Evaluate each path on its own merits against the ground truths. The "industry
standard" path may win -- but it should win because the analysis led there,
not because it was the default.

### Phase 5: Artifact Output

Produce a structured "First Principles Analysis" block in the chat.
This artifact stays in context and guides subsequent implementation.

**Quick artifact format:**

```markdown
## First Principles Analysis

**Problem:** [One sentence]
**Depth:** Quick

### Key Truths
- [Ground truth 1]
- [Ground truth 2]

### Assumptions Challenged

| Assumption | Challenge | Verdict |
|------------|-----------|---------|
| [What was assumed] | [Why question it] | Keep / Discard / Investigate |

### Recommended Approach
[Solution built from truths, with brief reasoning]
```

**Standard/Deep artifact format:**

```markdown
## First Principles Analysis

**Problem:** [One sentence]
**Depth:** Standard | Deep

### Ground Truths
- [TRUTH] [Description -- why this is irreducible]
- [TRUTH] [Description]

### Assumptions Challenged

| Assumption | Challenge | Verdict |
|------------|-----------|---------|
| [What was assumed] | [Why question it] | Keep / Discard / Investigate |

### Decomposition
[Component breakdown with truth/assumption tags]

### Recommended Approach
[Solution built ground-up from truths, with reasoning for each design choice]

### Alternative Paths
- **Path B:** [Approach] -- builds on [truths], trades off [X for Y]
- **Path C:** [Approach] -- builds on [truths], trades off [X for Y]

### Open Questions (if any)
- [Sub-problems noted but not fully decomposed]
```

---

## Examples

For worked examples showing the full flow across different domains, read
`references/first-principles-examples.md`. Examples include Redis caching decisions, microservices
evaluation, authentication system design, and database selection.

For real-world examples from SpaceX, Tesla, and other domains that build intuition
for the methodology, read `references/first-principles-real-world-examples.md`.

---

## Key Principles

- **Opinionated about process, never about solution.** Ruthlessly enforce the discipline
  of decomposition. Never skip to "just use X." But once ground truths are identified,
  present options and let the user choose.
- **Separate what IS from what we ASSUME.** The core skill is distinguishing irreducible
  constraints from inherited conventions. Everything flows from this distinction.
- **Recursive, not linear.** Problems nest. Sub-problems have their own assumptions.
  The skill handles depth, not just sequence.
- **Proportional effort.** Quick tasks get quick analysis. Deep problems get deep analysis.
  The skill should never feel like overhead for the wrong problem.
- **Build from bedrock upward.** Solutions are constructed from verified ground truths,
  not adapted from existing solutions. When the recommended path happens to match the
  conventional approach, that's fine -- but it should be because the analysis led there,
  not because it was the default.

---

## Common Traps

Watch for these patterns -- they signal that reasoning by analogy has crept in.

### The Analogy Trap
"Company X does it this way, so we should too."

**First Principles Check:** Is your problem identical to theirs in all relevant
dimensions? What constraints did they have that you don't? What constraints do
you have that they didn't?

### The Complexity Trap
Solution is more complex than the problem warrants.

**First Principles Check:** Remove one component at a time. If the system still
solves the core problem without it, that component wasn't essential. Repeat until
removal breaks core functionality.

### The Legacy Trap
Maintaining compatibility with decisions that no longer serve the current system.

**First Principles Check:** What was the original reason for this decision? Do
those conditions still exist? What's the true cost of changing vs. the ongoing
cost of maintaining?

### The Tool Trap
"We have X, so everything looks like an X problem."

**First Principles Check:** Would you choose this tool if you were starting
fresh today with no sunk cost? Is the tool driving the design, or is the
problem driving the tool choice?

### When First Principles Thinking Is Overkill

First principles thinking itself can be misapplied. An additional ground truth
to always keep in mind: **development time is finite and expensive.**

- If an existing solution is within 2x of optimal and the team already knows it,
  use it. First principles is most valuable when existing solutions are 10x wrong.
- For MVP or fast-iteration contexts, the conventional path is often correct.
- Challenge assumptions, but respect domain complexity -- first principles without
  domain expertise leads to naive solutions.

---

## Integration Notes

- **Standalone:** This skill does not automatically invoke other skills when done.
  The artifact feeds naturally into whatever the user does next.
- **Composable:** Other skills (brainstorming, writing-plans) can invoke this skill
  during their analysis phases.
- **Overridable:** The user can always say "skip the analysis" or "just do it" to
  bypass the skill entirely.

---

## Boundaries

**This skill will:**
- Challenge assumptions systematically
- Identify ground truths and separate them from conventions
- Build reasoning chains from fundamentals
- Reveal when conventional wisdom doesn't fit the current context

**This skill will not:**
- Dismiss all existing solutions as wrong (sometimes the conventional path IS the right one)
- Spend unlimited time on every decision (proportional effort is a core principle)
- Ignore practical constraints in favor of theoretical purity
- Guarantee the "best" solution (it reveals better reasoning, not perfect answers)
- Re-derive everything from scratch when an existing solution is good enough

---

## Quick Reference

- [ ] Problem stated in terms of outcomes, not solutions
- [ ] All assumptions explicitly listed and categorized
- [ ] Each assumption challenged with a verdict (Keep / Discard / Investigate)
- [ ] Ground truths verified with the three-question test
- [ ] Solution built upward from ground truths only
- [ ] Every design choice traceable to a ground truth
- [ ] Trade-offs acknowledged and documented
- [ ] Reasoning chain documented in the artifact
