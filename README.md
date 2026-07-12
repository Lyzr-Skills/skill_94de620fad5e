<h1 align="center">First Principles Thinking Skill</h1>

<p align="center">
  <b>Stop solving the wrong problem efficiently</b><br>
  A coding agent skill that challenges assumptions, identifies ground truths, and builds solutions from bedrock rather than convention.
</p>

<p align="center">
  <sub>5 phases &middot; 3 depth levels &middot; 5 platforms</sub>
</p>

<h4 align="center">
  <a href="#installation">Installation</a> |
  <a href="#usage">Usage</a> |
  <a href="references/first-principles-examples.md">Examples</a> |
  <a href="#contributing">Contributing</a>
</h4>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT License" /></a>
  <a href="https://github.com/swapnildahiphale/first-principles-thinking-skill/stargazers"><img src="https://img.shields.io/github/stars/swapnildahiphale/first-principles-thinking-skill?style=social" alt="GitHub Stars" /></a>
  <a href="https://github.com/swapnildahiphale/first-principles-thinking-skill/network/members"><img src="https://img.shields.io/github/forks/swapnildahiphale/first-principles-thinking-skill?style=social" alt="GitHub Forks" /></a>
  <a href="https://github.com/swapnildahiphale/first-principles-thinking-skill/pulls"><img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs Welcome" /></a>
</p>

<p align="center">
  <sub>Works with Claude Code &middot; Cursor &middot; Codex &middot; OpenCode &middot; Gemini CLI</sub>
</p>

First principles thinking is a problem-solving method that breaks engineering decisions into fundamental truths and rebuilds solutions from the ground up -- rather than copying patterns by analogy. This coding agent skill brings that methodology into your daily workflow, automatically challenging assumptions before you commit to an architecture, a technology choice, or a debugging strategy.

## What Is First Principles Thinking?

Instead of reasoning by analogy ("Netflix uses microservices, so we should too"), first principles thinking asks: *what are the actual constraints, and what solution follows from those constraints alone?*

This skill applies that discipline to software engineering. Instead of letting you jump straight to "just use Redis" or "let's do microservices," it forces a structured decomposition:

1. **Restates the problem** and confirms understanding
2. **Asks Socratic questions** to surface hidden assumptions
3. **Decomposes** into ground truths vs inherited conventions
4. **Rebuilds solutions** from verified truths upward
5. **Produces a structured artifact** that guides implementation

The skill auto-detects when you're making architecture, design, or technology decisions and activates proportionally -- quick analysis for medium decisions, deep analysis for system design.

## Why First Principles Thinking?

Most engineering mistakes don't come from bad execution. They come from solving the wrong problem, or solving the right problem with inherited assumptions baked in.

- **"We need a cache"** -- Do you? Or is the query just badly written?
- **"Let's use microservices"** -- Why? What's the actual scaling constraint?
- **"We should use Kafka"** -- For 100 messages per second?

First principles thinking catches these assumptions before they become architecture. It's the difference between building the right thing and building the wrong thing efficiently.

## Installation

### Claude Code

Register the GitHub repo as a marketplace, then install:

```bash
/plugin marketplace add swapnildahiphale/first-principles-thinking-skill
/plugin install first-principles-thinking@swapnildahiphale
```

Claude Code reads `.claude-plugin/marketplace.json` from the repo to discover the plugin.

### Cursor

Clone the repo and add the plugin locally:

```bash
git clone https://github.com/swapnildahiphale/first-principles-thinking-skill.git
```

Then in Cursor Settings > Features > Skills, add the path to the cloned directory. Or copy `SKILL.md` and `references/` into your project's `.cursor/skills/` directory.

### Codex

Tell Codex:

```
Fetch and follow instructions from https://raw.githubusercontent.com/swapnildahiphale/first-principles-thinking-skill/refs/heads/main/.codex/INSTALL.md
```

Or install manually:

```bash
git clone https://github.com/swapnildahiphale/first-principles-thinking-skill.git ~/.codex/first-principles-thinking
mkdir -p ~/.agents/skills
ln -s ~/.codex/first-principles-thinking ~/.agents/skills/first-principles-thinking
```

Restart Codex. The skill is discovered automatically from `~/.agents/skills/`.

### OpenCode

Add to the `plugin` array in your `opencode.json` (global or project-level):

```json
{
  "plugin": ["first-principles-thinking@git+https://github.com/swapnildahiphale/first-principles-thinking-skill.git"]
}
```

Restart OpenCode. The skill auto-registers on launch.

### Gemini CLI

```bash
gemini extensions install https://github.com/swapnildahiphale/first-principles-thinking-skill
```

### Manual Installation (any platform)

Clone and symlink or copy into your platform's skills directory:

```bash
git clone https://github.com/swapnildahiphale/first-principles-thinking-skill.git
```

Copy `SKILL.md` and `references/` into whichever directory your agent scans for skills.

### Verify Installation

Start a new session and try a problem that should trigger the skill, for example: "Should I use Redis or Memcached for caching?" The agent should activate first-principles analysis before recommending a solution.

## Usage

The skill triggers automatically when it detects complexity signals in your request:

| Trigger Type | Examples |
|-------------|---------|
| Architecture | "how should I structure this?", "what pattern should I use?" |
| Technology selection | "should I use Redis or Memcached?", "which database?" |
| Complex debugging | "can't figure out why this keeps failing" |
| Performance | "this endpoint is slow, should I add caching?" |
| Explicit | "think from first principles", "FP mode" |

You can also invoke it manually with phrases like "first principles" or "FP mode."

### Depth Levels

| Level | When | Time |
|-------|------|------|
| Quick | Medium decisions, manual invoke | ~1-2 min |
| Standard | Architecture, design, tech selection | ~3-5 min |
| Deep | System design, hard debugging | ~5-10 min |

### Example

```
You: "Our API is slow. Should we add a Redis cache?"

Skill: Restates problem, then asks:
  - "Have you profiled where the time is actually spent?"
  - "What's the current latency vs target?"
  - "What if the slowness is in the query, not the lack of a cache?"

Decomposes into:
  [TRUTH] P99 must be under 200ms (SLA)
  [TRUTH] Current: 800ms average
  [ASSUMPTION] We need caching -> Maybe fix the query first
  [ASSUMPTION] Redis specifically -> Memoization might suffice

Recommends: Fix the N+1 query first (2 hours, trivial to revert).
Measure again. Only add caching if still needed.
```

## Updating

```bash
# Claude Code
/plugin update first-principles-thinking

# Codex
cd ~/.codex/first-principles-thinking && git pull

# OpenCode (updates automatically on restart)

# Gemini CLI
gemini extensions update first-principles-thinking
```

## Structure

```
first-principles-thinking-skill/
├── SKILL.md                     # Main skill definition
├── references/
│   ├── first-principles-examples.md              # 4 worked examples
│   └── first-principles-real-world-examples.md   # SpaceX/Tesla examples
├── docs/                        # GitHub Pages landing page
│   ├── index.md
│   └── _config.yml
├── assets/
│   └── social-preview.png       # Social media preview image
├── .claude-plugin/
│   ├── plugin.json              # Claude Code plugin manifest
│   └── marketplace.json         # Marketplace catalog
├── .cursor-plugin/
│   └── plugin.json              # Cursor plugin manifest
├── .codex/
│   └── INSTALL.md               # Codex installation instructions
├── README.md
└── LICENSE
```

## Key Concepts

**Ground Truth vs Assumption** -- The core distinction. Ground truths are constrained by physics, math, or verified requirements. Assumptions are inherited conventions that can be challenged.

**Smart Threshold** -- Auto-triggers for complex decisions, stays dormant for simple tasks. You can always say "just do it" to bypass.

**Proportional Depth** -- Quick/Standard/Deep levels match the analysis effort to the problem's importance.

**Common Traps** -- Named patterns to watch for: Analogy Trap, Complexity Trap, Legacy Trap, Tool Trap.

## Contributing

Contributions welcome. Please open an issue or pull request.

## Creator

<table>
  <tr>
    <td>
      <strong>Swapnil Dahiphale</strong> &middot; SRE &middot; Builder<br>
      <em>"Built by someone who questions everything."</em>
    </td>
    <td align="right">
      <a href="https://swapnil.one">
        <img src="https://img.shields.io/badge/Portfolio-000000?style=for-the-badge&logo=safari&logoColor=white" alt="Portfolio" />
      </a>&nbsp;
      <a href="https://www.linkedin.com/in/swapnil2233/">
        <img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn" />
      </a>
    </td>
  </tr>
</table>

## License

MIT License -- see [LICENSE](LICENSE) for details.
