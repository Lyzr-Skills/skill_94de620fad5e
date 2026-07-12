---
layout: default
title: "First Principles Thinking Skill - Challenge Assumptions, Find Ground Truths"
description: "A coding agent skill that applies first principles thinking to engineering problems. Works with Claude Code, Cursor, Codex, OpenCode, and Gemini CLI."
---

# First Principles Thinking for Coding Agents

**Stop solving the wrong problem efficiently.** This skill brings first principles thinking into your coding agent -- systematically challenging assumptions, identifying ground truths, and building solutions from bedrock rather than convention.

## What is first principles thinking?

First principles thinking is a problem-solving method rooted in Aristotle's philosophy and popularized by Elon Musk. Instead of reasoning by analogy ("Netflix uses microservices, so we should too"), you break a problem down to its fundamental truths and reason upward from there.

Applied to software engineering, this means questioning every inherited convention before committing to an architecture, a technology choice, or a debugging strategy.

## How the skill works

The skill runs five phases when it detects a complex engineering decision:

1. **Problem Intake** -- Restates and confirms the problem before doing anything else
2. **Socratic Questioning** -- Challenges your assumptions using six types of probing questions
3. **Decomposition** -- Separates ground truths (physics, math, verified requirements) from assumptions (inherited conventions)
4. **Reconstruction** -- Builds 2-3 solution paths exclusively from verified ground truths
5. **Artifact Output** -- Produces a structured analysis you can reference during implementation

### Example

```
You: "Our API is slow. Should we add a Redis cache?"

Skill decomposes into:
  [TRUTH] P99 must be under 200ms (SLA requirement)
  [TRUTH] Current average: 800ms
  [ASSUMPTION] We need caching → Maybe the query is just badly written
  [ASSUMPTION] Redis specifically → In-memory memoization might suffice

Recommends: Fix the N+1 query first. Measure again.
Only add caching if still needed.
```

## Why first principles thinking matters in engineering

Most engineering mistakes don't come from bad execution. They come from solving the wrong problem, or solving the right problem with inherited assumptions baked in.

| Common assumption | First principles question |
|---|---|
| "We need a cache" | Have you profiled where the time is spent? |
| "Let's use microservices" | What's the actual scaling constraint? |
| "We should use Kafka" | For 100 messages per second? |
| "Let's rewrite in Rust" | Where is the bottleneck? Is it CPU-bound? |

## Supported platforms

This skill works with any coding agent that supports skills or plugins:

- **Claude Code** -- Install via plugin marketplace
- **Cursor** -- Clone and add to skills directory
- **Codex** -- Symlink to `~/.agents/skills/`
- **OpenCode** -- Add to `opencode.json` plugins
- **Gemini CLI** -- Install as extension

## Install

Visit the [GitHub repository](https://github.com/swapnildahiphale/first-principles-thinking-skill) for detailed installation instructions for each platform.

### Quick start (Claude Code)

```bash
/plugin marketplace add swapnildahiphale/first-principles-thinking-skill
/plugin install first-principles-thinking@swapnildahiphale
```

## Key concepts

**Ground Truth vs Assumption** -- Ground truths are constrained by physics, math, or verified requirements. Assumptions are inherited conventions that can be challenged.

**Smart Threshold** -- The skill auto-triggers for complex decisions (architecture, technology selection, hard debugging) and stays dormant for simple tasks.

**Depth Levels** -- Quick (~1-2 min), Standard (~3-5 min), and Deep (~5-10 min) analysis levels match effort to problem importance.

**Common Traps** -- Named anti-patterns the skill watches for: Analogy Trap, Complexity Trap, Legacy Trap, Tool Trap.

## Links

- [GitHub Repository](https://github.com/swapnildahiphale/first-principles-thinking-skill)
- [Worked Examples](https://github.com/swapnildahiphale/first-principles-thinking-skill/blob/main/references/first-principles-examples.md)
- [Real-World Examples (SpaceX, Tesla)](https://github.com/swapnildahiphale/first-principles-thinking-skill/blob/main/references/first-principles-real-world-examples.md)

---

Built by [Swapnil Dahiphale](https://swapnil.one) -- SRE, builder, someone who questions everything.
