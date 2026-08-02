# DeepCell plugins for Claude Code

```
/plugin marketplace add deepcell-ai/deepcell-plugins
/plugin install deepcell@deepcell
```

The plugin drives the `deepcell` CLI rather than bundling it. Install it and
sign in first — full instructions, including whether to use the CLI or the MCP
server, are at <https://beta.deepcell.net/product/claude-code>, and in plain
markdown for an agent at <https://beta.deepcell.net/product/claude-code.md>.

## About this repository

This repository is a **published build artifact**, not the source of truth. Its
contents are generated from the DeepCell repository, where the plugin's skill and
agent are in turn generated from the prompts the hosted DeepCell agent runs on —
so the plugin can never drift from the product.

That means pull requests here cannot be merged: every publish overwrites this
tree. Bug reports and requests are welcome as **issues on this repository** —
they are read, and a fix lands upstream and arrives here on the next publish —
or by mail to <hello@deepcell.net>.
