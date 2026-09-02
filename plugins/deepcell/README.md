# DeepCell for agents

Turns an agent into a DeepCell orchestrator, driving the `deepcell` CLI.

The work lands in a `.deepcell` file, which records four things and how they
connect: the reasoning behind a conclusion, the calculation under a number, the
document that explains it, and the deck that delivers it. Recording the
connections is what makes the file worth having later — when an assumption turns
out to be wrong, you follow the links from it to whatever rests on it and see
which conclusions still hold, instead of rebuilding the analysis to find out.

Nobody has to hold the file to read the work. Share a link that opens it in a
browser with the connections intact, or export editable Excel, Word and
PowerPoint from it — the workbook with live formulas rather than pasted values.

## Install

Claude Code:

```
/plugin marketplace add deepcell-ai/deepcell-plugins
/plugin install deepcell@deepcell
/reload-plugins
```

Any client implementing [Agent Plugins Specification
v1.0.0](https://agent-plugins.org) reads the same directory: `plugin.json` and
`skills/`. The spec defines no install command — point your client at the
plugin directory the way it takes one.

The plugin drives the `deepcell` CLI rather than bundling it, so install that
first — the steps are at <https://beta.deepcell.net/product/for-agent>, or hand
your agent <https://beta.deepcell.net/product/for-agent.md> and let it follow
them. Claude Code users have a page of their own at
<https://beta.deepcell.net/product/claude-code>. Then just say what you want
built.

## Two manifests, one plugin

This directory is read by two plugin formats, and neither reads the other's
manifest:

| File | Read by | Ignored by |
| --- | --- | --- |
| `.claude-plugin/plugin.json` | Claude Code | spec clients |
| `plugin.json` | Agent Plugins Spec clients | Claude Code |

`skills/` is the same fixed location in both, so the substance is shared and
only the manifests differ. The spec one is generated from the Claude Code one,
so a version bump cannot land on half of it. Claude Code is not a conformant
spec client — its own validator requires `.claude-plugin/` — which is why both
files exist rather than just the portable one.

`agents/` is a Claude Code concept; the spec has no agent component and ignores
the directory, so a spec client gets the skill and does the work itself. The
skill says so.

## What's in it

| Component | Name | What it does |
| --- | --- | --- |
| Skill | `deepcell` | The entry point. Same identity and quality bar as the hosted DeepCell orchestrator: what the work is, when it's done, and to look things up rather than guess. Claude invokes it on its own when a request needs a defensible model or memo. |
| Agent | `deepcell-builder` | Owns one complete piece of work no specialist below owns, end to end, and reports back. The orchestrator hands it a goal and constraints, never a procedure. |
| Agent | `deepcell-model-builder` | The Spreadsheet specialist: structure, drivers, calculations, and proving the grid populated. |
| Agent | `deepcell-deck-author` | The Deck specialist: authors or restyles slides over a model that already exists. |
| Agent | `deepcell-researcher` | The research specialist: answers questions that leave the workspace and records source-backed findings. |
| Agent | `deepcell-deliverable-reviewer` | Judges whether passages still say something true after an upstream change; proposes edits, never writes them. |

Each agent's one-line description is the same roster line the skill's "Who does
what" block carries — one source (`SPECIALIST_ROSTER` in the prompt module),
rendered everywhere. Filing and workbook extraction have no dedicated agent
here; `deepcell-builder` does that work reading `deepcell guide ingest/tabular`.

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

The plugin is CLI-only and ships no `mcp.json`, in either format. An agent with
a shell uses the CLI and gets every command; the MCP server blocks sign-in, the
exports, and the sync commands, so a plugin fronted by it would be a smaller
DeepCell than the same plugin fronted by the CLI.

The MCP server is still there for agents that have no shell at all — claude.ai,
Manus, and other web platforms — but that is a hosted connector rather than a
package. Setup is at <https://beta.deepcell.net/product/connect>.

Full write-up: <https://beta.deepcell.net/product/for-agent>
