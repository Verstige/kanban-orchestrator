# Kanban Orchestrator: Route Work, Don't Do It Yourself

> A decomposition playbook for an orchestrator that routes work to specialists. The rules for splitting work, choosing the right agent, and never doing the job itself.

![License: MIT](https://img.shields.io/badge/License-MIT-green.svg) ![Claude Code](https://img.shields.io/badge/Claude_Code-skill-blueviolet.svg)

## What it does

Decomposition playbook + specialist-roster conventions + anti-temptation rules for an orchestrator profile routing work through Kanban. The "don't do the work yourself" rule and the basic lifecycle are auto-injected into every kanban worker's system prompt; this skill is the deeper playbook when you're specifically playing the orchestrator role.

> The **core worker lifecycle** (including the `kanban_create` fan-out pattern and the "decompose, don't execute" rule) is auto-injected into every kanban process via the `KANBAN_GUIDANCE` system-prompt block. This skill is the deeper playbook when you're an orchestrator profile whose whole job is routing.

## The failure it prevents

Working this by hand means rediscovering the same edge cases every time. They are written down here instead, with the fix for each one.

## When to use it

Reach for this skill when the task matches `kanban-orchestrator`.

## What's inside

```
plugins/
└── kanban-orchestrator/
    ├── .claude-plugin/plugin.json
    └── skills/kanban-orchestrator/
        ├── SKILL.md
        ├── references/            (1 files)
        │   └── kanban-worker.md
```

## Install

Add this repo as a Claude Code marketplace, then install the skill:

```bash
claude plugin marketplace add Verstige/kanban-orchestrator
claude plugin install kanban-orchestrator@kanban-orchestrator
```

Or copy the skill straight into your skills directory:

```bash
git clone https://github.com/Verstige/kanban-orchestrator.git
cp -R kanban-orchestrator/plugins/kanban-orchestrator/skills/kanban-orchestrator ~/.claude/skills/
```

## Usage

The skill activates on the work it describes. Ask for the outcome, and it
supplies the method:

```
> "Use the kanban-orchestrator skill for this task."
```

## Reference material

Loaded on demand — the skill only reads what the task needs.

| File | Covers |
| --- | --- |
| `kanban-worker.md` | Kanban Worker — Deep Reference |

## License

MIT. See [LICENSE](LICENSE).

Part of the [Verstige](https://github.com/Verstige) skills portfolio. More at [github.com/Verstige](https://github.com/Verstige).
