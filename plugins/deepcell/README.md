# DeepCell for Claude Code

Turns Claude Code into a DeepCell orchestrator. The work lands in a `.deepcell`
file — a model or a memo that keeps the assumptions, evidence, and math behind
its conclusions — and Claude drives it through the `deepcell` CLI.

## Install

```
/plugin marketplace add fimo-copilot/deepcell-plugins
/plugin install deepcell@deepcell
/reload-plugins
```

The plugin drives the `deepcell` CLI rather than bundling it, so install that
and sign in — the steps are at <https://beta.deepcell.net/product/claude-code>,
or hand your agent <https://beta.deepcell.net/product/claude-code.md> and let it
follow them. Then just say what you want built.

## What's in it

| Component | Name | What it does |
| --- | --- | --- |
| Skill | `deepcell` | The entry point. Same identity and quality bar as the hosted DeepCell orchestrator: what the work is, when it's done, and to look things up rather than guess. Claude invokes it on its own when a request needs a defensible model or memo. |
| Agent | `deepcell-builder` | Owns one complete piece of work end to end and reports back. The orchestrator hands it a goal and constraints, never a procedure. |

## How it knows things

Three layers, and the plugin owns only the first:

- **These instructions** — identity, the quality bar, judgment.
- **`deepcell guide <topic>`** — how the format and the modeling work.
- **`deepcell --help`** — the exact commands, flags, and arguments.

Nothing here names a flag, a tag, or a function, so nothing here goes stale when
one is renamed. The skill and the agent are generated from
`backend/src/agents/deepcell/prompts.py` — the same prompts the hosted agent
runs on — by `scripts/gen_claude_plugin.py`, and a CI drift test fails if they
fall out of sync. Edit the Python module, regenerate, commit both.

## CLI or MCP?

Claude Code has a shell, so it uses the CLI and gets every command. The MCP
server is for agents that have no shell (claude.ai, Manus, and other web
platforms); it blocks sign-in, the exports, and the sync commands. Setup for
that path is at <https://beta.deepcell.net/product/connect>.

Full write-up: <https://beta.deepcell.net/product/claude-code>
