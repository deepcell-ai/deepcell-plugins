# DeepCell plugins

## Claude Code

```
/plugin marketplace add deepcell-ai/deepcell-plugins
/plugin install deepcell@deepcell
```

## Any Agent Plugins Spec client

This repository root is also a plugin conforming to
[Agent Plugins Specification v1.0.0](https://agent-plugins.org) — `plugin.json`
and `skills/`. The spec does not define an install command, so point your
client at this repository the way it takes a plugin directory. The same skill is
also at `plugins/deepcell/` for clients that address a subdirectory; the two
copies are identical.

The plugin ships one skill and no MCP server: it drives the `deepcell` CLI
rather than bundling it, so install the CLI first. Full instructions are at
<https://beta.deepcell.net/product/for-agent>, in plain markdown for an agent at
<https://beta.deepcell.net/product/for-agent.md>, and for Claude Code
specifically at <https://beta.deepcell.net/product/claude-code>.

## About this repository

This repository is a **published build artifact**, not the source of truth. Its
contents are generated from the DeepCell repository, where the plugin's skill and
agent are in turn generated from the prompts the hosted DeepCell agent runs on —
so the plugin can never drift from the product.

That means pull requests here cannot be merged: every publish overwrites this
tree. Bug reports and requests are welcome as **issues on this repository** —
they are read, and a fix lands upstream and arrives here on the next publish —
or by mail to <hello@deepcell.net>.
