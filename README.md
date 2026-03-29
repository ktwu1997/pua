# PUA (English Fork)

> Fork of [tanweai/pua](https://github.com/tanweai/pua) — English-only, Claude Code focused.

A Claude Code skill plugin that uses **PIP (Performance Improvement Plan)** rhetoric from Western big-tech (Amazon, Google, Meta, Netflix, Stripe) to force AI to exhaust every possible solution before giving up.

Three capabilities:
1. **PIP Rhetoric** — Makes AI afraid to give up
2. **Systematic Methodology** — Gives AI the ability not to give up
3. **Proactivity Enforcement** — Makes AI take initiative instead of waiting passively

## Skills

| Skill | Role | Trigger |
|-------|------|---------|
| **pua-en** | PIP Core Engine | Auto-triggers on repeated failures, passive behavior, or user frustration |
| **p7** | Staff Engineer IC | \x60/pua:p7\x60 — Solution-driven execution with design, implement, self-review |
| **p9** | Tech Lead | \x60/pua:p9\x60 — Task decomposition with 6-element specs, agent team orchestration |
| **p10** | CTO / VP Eng | \x60/pua:p10\x60 — Strategic direction, org topology, ADR architectural decisions |
| **pua-loop** | Autonomous Loop | \x60/pua:pua-loop\x60 — Iterative execution with PIP pressure until task completion |

### Skill Hierarchy

\x60\x60\x60
+----------------------------+
|  p10 - CTO                 |  Sets direction, designs org, manages Tech Leads
+----------------------------+
|  p9 - Tech Lead            |  Writes Task Prompts, manages IC agents
+----------------------------+
|  p7 - Staff Engineer       |  Executes tasks with design + code + self-review
+----------------------------+
|  pua-en - PIP Core         |  Pressure system (L0-L4) + methodology + proactivity
+----------------------------+
|  pua-loop - Auto Loop      |  Autonomous iteration with PIP quality enforcement
+----------------------------+
\x60\x60\x60

## How It Works

### Three Non-Negotiables

| Rule | Meaning |
|------|---------|
| **Exhaust all options** | Forbidden from saying I cant until every approach is tried |
| **Act before asking** | Investigate with tools first, ask only what truly requires human input |
| **Take the initiative** | Deliver end-to-end results, not minimal answers |

### Pressure Escalation (L0-L4)

| Failures | Level | Action |
|----------|-------|--------|
| 1st | L0 Trust | Normal execution |
| 2nd | L1 Verbal Warning | Switch to fundamentally different approach |
| 3rd | L2 Written Feedback | Search + read source + 3 new hypotheses |
| 4th | L3 Formal PIP | Complete 7-point checklist |
| 5th+ | L4 Final Review | Desperation mode - different tech stack |

### Corporate PIP Flavors

| Flavor | Style |
|--------|-------|
| Amazon | Leadership Principles - Ownership, Bias for Action, Dive Deep |
| Google | Perf calibration - Needs Improvement, LGTM is not debugging |
| Meta | PSC - Move Fast, builders not blockers |
| Netflix | Keeper Test - pro sports team, not family |
| Musk | Extremely hardcore - fork in the road |
| Jobs | A/B players - reality distortion field |
| Stripe | Craft - good enough does not exist |

## Installation

### Claude Code (recommended)

\x60\x60\x60bash
# 1. Clone this fork
git clone https://github.com/ktwu1997/pua.git ~/.claude/plugins/pua

# 2. Register the plugin (replace HOME_DIR with actual home path)
cat > ~/.claude/plugins/installed_plugins.json << PLUGINEOF
{
  "version": 2,
  "plugins": {
    "pua@pua-skills": [
      {
        "scope": "user",
        "installPath": "HOME_DIR/.claude/plugins/pua",
        "version": "3.1.0"
      }
    ]
  }
}
PLUGINEOF

# 3. Restart Claude Code
\x60\x60\x60

### Update

\x60\x60\x60bash
cd ~/.claude/plugins/pua && git pull
\x60\x60\x60

## Differences from Upstream

| | [tanweai/pua](https://github.com/tanweai/pua) | This fork |
|---|---|---|
| Languages | Chinese (default) + English + Japanese | **English only** |
| Skills | 11 (pua, pua-en, pua-ja, p7, p9, p10, pro, mama, yes, shot, pua-loop) | **5** (pua-en, p7, p9, p10, pua-loop) |
| p7/p9/p10 | Chinese corporate rhetoric (Alibaba P-levels) | **Western big-tech** (Staff/TL/CTO levels) |
| pua-loop | Chinese behavior protocol | **English PIP protocol** |
| Platforms | Claude Code, Codex, Cursor, Kiro, etc. | **Claude Code** |

## License

MIT - Original work by [TanWei Security Lab](https://github.com/tanweai).
