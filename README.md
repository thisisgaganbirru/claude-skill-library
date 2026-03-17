[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Status: Active](https://img.shields.io/badge/Status-Active-brightgreen?style=flat)]()

# Agent Skills

> A skill library for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that activates specialist expert personas on demand — so you get a battle-tested Scrum Master, principal systems architect, or README engineer instead of a general-purpose response.

Claude is a capable generalist, but generalist responses fall short when you need real domain depth: a sprint retro that actually surfaces dysfunction, a distributed system design with real trade-off analysis, or a README that passes the 30-second developer test. These skills solve that by giving Claude a structured expert identity — a full persona, decision framework, and domain knowledge base — that activates the moment you describe a relevant problem.

**Primary:** [Claude Code](https://docs.anthropic.com/en/docs/claude-code) · **Also works with:** claude.ai, Cursor, GitHub Copilot CLI, and any agent that supports the skills spec.

**Who it's for:** Engineers, tech leads, and content creators who want specialist-quality responses without switching tools.

---

## Table of Contents

- [Skills](#skills)
- [How Skills Work](#how-skills-work)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Skill Structure](#skill-structure)
- [Adding a New Skill](#adding-a-new-skill)
- [Contributing](#contributing)
- [License](#license)

---

## Skills

| Skill                                         | Description                                                                                          |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| [`claude-md`](./claude-md/)                   | Expert at creating, auditing, and optimizing `CLAUDE.md` project memory files for Claude agents      |
| [`content-strategist`](./content-strategist/) | Senior content strategist and SEO growth expert covering creators, brands, and platform growth       |
| [`readme-writer`](./readme-writer/)           | Senior-level README engineer that creates, audits, and enhances `README.md` files for any project    |
| [`scrum-master`](./scrum-master/)             | Battle-tested senior Scrum Master for sprint planning, ceremonies, metrics, and Agile coaching       |
| [`senior-tech-lead`](./senior-tech-lead/)     | Senior Technical Team Lead for people management, technical decisions, delivery, and crisis handling |
| [`system-design`](./system-design/)           | Principal-level distributed systems architect for designing, scaling, and reviewing any system       |

---

## How Skills Work

Each skill is a folder containing a `SKILL.md` file with YAML frontmatter (`name`, `description`) and an optional `references/` subdirectory of domain knowledge files. Claude reads the `description` and decides when to activate — there is no runtime routing layer, no build step, and no dependencies. Plain Markdown.

When a skill activates, Claude adopts the full persona defined in the skill body, applies its decision framework, and draws on the reference files for deep domain knowledge.

**Invocation:** No command or slash syntax needed. Just describe your problem naturally in the chat — Claude matches it against the loaded skill descriptions and activates the right one automatically. There is no explicit `/skill` command.

> [!TIP]
> If a skill does not activate, rephrase using language closer to the trigger conditions in that skill's `description` field — visible at the top of each `SKILL.md`.

---

## Prerequisites

| Tool                                                          | Version            | Notes                          |
| ------------------------------------------------------------- | ------------------ | ------------------------------ |
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code) | Latest             | Primary platform               |
| Bash / Zsh                                                    | Any modern version | macOS / Linux install commands |
| PowerShell                                                    | ≥ 5.1              | Windows install commands       |

> [!NOTE]
> Skills are plain Markdown — no build step, no dependencies, no package manager required.

---

## Installation

### Claude Code (Primary)

```bash
# macOS / Linux — install a single skill
cp -r scrum-master ~/.claude/skills/

# Install all skills at once
for skill in claude-md content-strategist readme-writer scrum-master senior-tech-lead system-design; do
  cp -r "$skill" ~/.claude/skills/
done
```

```powershell
# Windows PowerShell — install all skills at once
$skills = @("claude-md","content-strategist","readme-writer","scrum-master","senior-tech-lead","system-design")
foreach ($skill in $skills) {
  Copy-Item -Recurse $skill "$HOME\.claude\skills\$skill"
}
```

### Other Compatible Agents

The same skill folders work with **claude.ai** (upload as zip via Settings → Features → Skills), **Cursor**, **GitHub Copilot CLI** (`~/.copilot/skills/`), and any agent that supports the skills spec.

```bash
# GitHub Copilot CLI
cp -r scrum-master ~/.copilot/skills/
```

---

## Quick Start

Describe a problem the skill handles — the agent activates the right one automatically, no slash command required.

**`system-design` — ask a design question:**

```
Design a URL shortener that handles 100 million redirects per day.
```

Expected: Full architecture breakdown — capacity estimates, data model, component design, database selection rationale, and trade-off analysis.

**`scrum-master` — ask a sprint question:**

```
Our sprint review is tomorrow and we only finished 4 of 9 stories. How do I run it?
```

Expected: Structured facilitation plan — what to show stakeholders, how to frame incomplete work, retrospective setup, and velocity impact assessment.

**`senior-tech-lead` — ask a people or delivery question:**

```
Two senior engineers on my team are in a disagreement that's blocking a sprint. How do I resolve it?
```

Expected: Concrete facilitation steps, stakeholder framing, and a decision framework — not generic conflict advice.

---

## Skill Structure

Each skill follows the standard layout:

```
skill-name/
├── SKILL.md              # YAML frontmatter (name, description) + full persona body
└── references/           # Domain knowledge files loaded during skill execution
    ├── topic-one.md
    └── topic-two.md
```

The `SKILL.md` frontmatter drives activation:

```yaml
---
name: system-design
description: >
  Expert-level distributed systems design skill. Use this whenever the user asks
  to design, architect, review, or reason about any system...
---
```

The body defines the persona, decision framework, response patterns, and which reference files to load for each task type.

> [!NOTE]
> All skill folder and file names follow **kebab-case** (`^[a-z0-9-]+$`) as required by the skills spec.

---

## Adding a New Skill

1. Create a folder with a kebab-case name: `your-skill-name/`
2. Add `SKILL.md` with a YAML frontmatter block containing `name` and `description`, followed by the full persona and instruction body
3. Optionally add `references/` with supporting Markdown knowledge files
4. Copy the folder to `~/.claude/skills/` (Claude Code) or the equivalent for your agent

**Minimal `SKILL.md` structure:**

```markdown
---
name: your-skill-name
description: >
  Describe the persona and list the exact trigger phrases and request types
  that should activate this skill. Be specific — Claude uses this text
  to decide whether to activate.
---

# Skill Title

[Persona definition, response strategy, and reference file pointers]
```

The `description` field is the most important part — write it to match the language a user would naturally use when they need this skill.

---

## Contributing

Contributions are welcome. To add or improve a skill:

1. Fork the repository and create a branch: `git checkout -b skill/your-skill-name`
2. Follow the skill structure above — read an existing `SKILL.md` to understand the expected depth
3. Test activation by installing to your local skills directory and verifying the skill triggers on representative prompts
4. Open a pull request with a brief description of the skill's domain and trigger conditions

For bugs or improvements to existing skills, open an issue with the skill name in the title.

---

## License

This project is licensed under the **MIT License** (`SPDX: MIT`). See [LICENSE](LICENSE) for the full text.
