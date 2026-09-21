<h1 align="center">DeepCell</h1>

<p align="center"><strong>Stop making the same change in every file.</strong></p>

<p align="center">
A Claude Code plugin that builds your model, memo and deck as one <code>.deepcell</code> file —<br>
so when an assumption moves, the work moves with it, and you can see which conclusions still hold.
</p>

<p align="center">
  <a href="https://deepcell.net/product/claude-code">Setup guide</a> ·
  <a href="https://deepcell.net/demo">Try the demo</a> ·
  <a href="https://deepcell.net/story">What's in a file</a> ·
  <a href="https://deepcell.net/product/for-agent">Other agents</a> ·
  <a href="https://github.com/deepcell-ai/deepcell-plugins/issues">Issues</a>
</p>

<p align="center">
  <a href="https://pypi.org/project/deepcell-cli/"><img alt="deepcell CLI on PyPI" src="https://img.shields.io/pypi/v/deepcell-cli?label=deepcell%20CLI"></a>
  <a href="https://agent-plugins.org"><img alt="Agent Plugins Specification v1.0.0" src="https://img.shields.io/badge/Agent%20Plugins%20Spec-v1.0.0-informational"></a>
  <a href="https://github.com/deepcell-ai/deepcell-plugins/blob/main/LICENSE"><img alt="MIT license" src="https://img.shields.io/badge/license-MIT-lightgrey"></a>
</p>

---

## What it does

A valuation, a budget, a due-diligence memo, a board deck. Today each one is
its own file, and every change means finding the same number in all of them —
and hoping the reasoning that justified it is still somewhere.

Ask Claude Code for the work through DeepCell and it lands in **one
`.deepcell` file** that keeps four things and how they connect:

| Surface | What it holds |
| --- | --- |
| **Model** | The drivers and calculations, dependency-tracked — a growth rate, a cost ratio, a delivery date, and everything computed from them. |
| **Reasoning** | The thesis, the evidence behind it, the assumptions it rests on, the bear case and the counter — as a map you can follow end to end. |
| **Document** | The memo that explains the conclusion, citing the numbers rather than retyping them. |
| **Deck** | The slides that deliver it, bound to the same figures. |

Change one thing and DeepCell follows the links from it. What is certain updates
automatically; what needs your decision is marked for your call.

```
A supplier moves battery delivery from 8 weeks to 14.

  12 places depend on that delivery time — timeline, status report, review deck.
  10 update on their own.
   2 are yours:  Still commit to the June 30 launch?
                 Switch to the backup supplier?

  Product margin — unaffected.  Backup supplier qualification — unchanged.
```

Nobody needs an account, or the file, to read the work: share a link and it
opens in a browser with every connection intact. Or export editable **Excel,
Word and PowerPoint** — the workbook with live formulas, not pasted values.

Already used for valuations, budgets, supply-chain plans, research reports and
due diligence.

## Install

Inside Claude Code:

```
/plugin marketplace add deepcell-ai/deepcell-plugins
/plugin install deepcell@deepcell
/reload-plugins
```

The plugin drives the `deepcell` command-line tool rather than bundling it.
Ask Claude to install it, or run one line yourself — it is safe to run over a
copy that is already there:

```sh
curl -LsSf https://deepcell.net/install.sh | sh
```

```powershell
irm https://deepcell.net/install.ps1 | iex
```

There is no sign-in step before the first model: the tool works anonymously
until something needs an account. Updates, Windows notes and what to do when
`deepcell` is "not found" are on the setup page —
<https://deepcell.net/product/claude-code> — or hand your agent
<https://deepcell.net/product/claude-code.md> and let it follow along.

## Then say what you want built

A goal and its constraints, not a procedure. Claude reaches for the `deepcell`
skill on its own when a request needs a model or a memo that has to hold up;
`/deepcell:deepcell` calls it directly.

> Build a three-year DCF for Acme from the filings in `./filings`. Keep the
> growth and margin assumptions where I can change them, and record why each
> one is what it is.

> The Atlas subcontract cost ratio could land anywhere from 32% to 45%. Which
> of our FY26 conclusions survive the top of that range, and does the group
> still clear the board's 10% EBITDA floor?

> There's a report that Nvidia will backstop $250B of OpenAI's data-center
> financing. Treat it as an unconfirmed secondary source, weigh it against the
> quarterly results, and revise only the conclusions that depend on externally
> funded capex.

> Turn this budget workbook into a model, write the one-page summary for the
> steering committee, and build the six slides for Thursday.

The first thing it does is look — `deepcell --help` for the commands,
`deepcell guide` for the format and the modelling — rather than guess a flag
or a tag. What comes back is a link to the finished work, with every edit
versioned so you can see what changed, why, and go back.

## Come back when something changes

Same door. *Rates moved 50bp. The client pushed close to Q3. The new quarter is
out.* DeepCell finds every figure, claim and slide that depends on what moved:
dependent calculations recompute, every passage and slide that cited them is
checked and edits proposed, and what needs judgment is listed rather than
decided for you. The conclusions that still hold say so; the one that no longer
does is rewritten with the reason attached.

## What's in the plugin

| Component | Name | What it does |
| --- | --- | --- |
| Skill | `deepcell` | The entry point. Same identity and quality bar as the hosted DeepCell orchestrator: what the work is, when it's done, and to look things up rather than guess. |
| Agent | `deepcell-builder` | Owns one complete piece of work end to end — a filing to ingest, a workbook to convert, a model nobody below owns — and reports back. Gets a goal and constraints, never a procedure. |
| Agent | `deepcell-model-builder` | The spreadsheet specialist: structure, drivers, calculations, and proving the grid populated. |
| Agent | `deepcell-deck-author` | The deck specialist: authors or restyles slides over a model that already exists. |
| Agent | `deepcell-researcher` | The research specialist: answers questions that leave the workspace — diligence, a proposal, a market — and records source-backed findings in the file. |
| Agent | `deepcell-deliverable-reviewer` | Judges whether passages still say something true after an upstream change; proposes edits, never writes them. |

Claude Code launches a plugin's agents as `<plugin>:<agent>`, so the name the
Task tool accepts is `deepcell:deepcell-researcher` — the skill's roster prints
exactly those names.

## How it stays current

Three layers, and the plugin owns only the first:

- **These instructions** — identity, the quality bar, judgment.
- **`deepcell guide <topic>`** — how the format and the modelling work.
- **`deepcell --help`** — the exact commands, flags and arguments.

Nothing here names a flag, a tag or a function, so nothing here goes stale
when one is renamed. The skill and the agents are generated from the same
instructions the hosted DeepCell agent runs on, and a drift test fails if they
fall out of sync — which is why the plugin cannot say something the product
does not.

## Not on Claude Code?

**Any agent with a shell** can use the same plugin. This directory is also a
plugin under [Agent Plugins Specification v1.0.0](https://agent-plugins.org):
`plugin.json` and `skills/`. The spec defines no install command, so point your
client at the plugin directory the way it takes one; in the published
repository the root and `plugins/deepcell/` each carry an identical copy.
Claude Code reads its own manifest in `.claude-plugin/` and ignores the
portable one; a spec client does the reverse. The agents above are a Claude
Code concept and do not travel — a spec client gets the skill and does the
work itself. Setup for the general case:
<https://deepcell.net/product/for-agent>, in plain markdown for the agent at
<https://deepcell.net/product/for-agent.md>.

**No shell at all** — claude.ai, Manus and other web platforms — use the MCP
server instead. That is a hosted connector rather than a package, and it
blocks the commands that need a browser or a working copy (sign-in, the
exports, the sync commands), which is why the plugin ships no `mcp.json`: an
agent with a shell gets the whole of DeepCell through the CLI. Setup is at
<https://deepcell.net/product/connect>.

## About this repository

This repository is a **published build artifact**, not the source of truth.
Its contents are generated from the DeepCell repository, where the plugin's
skill and agents are in turn generated from the instructions the hosted agent
runs on — so the plugin can never drift from the product.

That means pull requests here cannot be merged: every publish overwrites this
tree. Bug reports and requests are welcome as **issues on this repository** —
they are read, and a fix lands upstream and arrives here on the next publish —
or by mail to <hello@deepcell.net>.
