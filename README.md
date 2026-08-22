# dsh-adversarial-review

Adversarial review for code, MCP configs and system prompts — every finding carries a repro path and confidence; unreproducible ones are marked suspected, never confirmed.

## What & why

Not a scanner: it reviews code/config/prompts YOU provide, it does not attack third-party services. `adversarial_review` probes named surfaces (injection, trust boundaries, secret handling, side effects, idempotency, swallowed errors) — each finding with a repro path and high/medium/low confidence, and anything it cannot reproduce is downgraded to *suspected*, not presented as confirmed (the same honesty contract as our tables' arithmetic self-check). `mcp_config_audit` checks agent/MCP configs for over-broad grants, embedded secrets, unannotated destructive tools. `red_team_prompt` attacks your own system prompt and hands back hardening. Pass `model` to run on a flagship (Claude/Kimi K3/GPT via AllRouter).

Start with `what_can_you_do` — describe your task in any language, get the exact tool and a ready-to-run call.

<!-- TOOLS:BEGIN -->
## What's in this pack

10 tools, read from the live endpoint on 2026-08-22 — **this table is generated, not hand-written**, so it cannot drift away from what `tools/list` actually returns. The **Arguments** column is what the tool genuinely reads; it comes from the tool's own declared input schema.

| Tool | What it does | Arguments | Price / call |
|---|---|---|---|
| `what_can_you_do` | Describe a task in plain language (any language) and get back exactly which tools on this server do it, with ready- | `task` | — |
| `adversarial_review` | Adversarial code/config/design review. Probes named attack surfaces (injection, trust boundaries, secret handling,  | `code`, `text`, `diff`, `config`, `model`, `language` | $0.03 |
| `mcp_config_audit` | Audit an MCP / agent config (JSON/YAML) for risk: over-broad tool permissions, destructive tools not annotated, sec | `config`, `text`, `code`, `model` | $0.02 |
| `red_team_prompt` | Red-team YOUR OWN system prompt: given a system/instruction prompt, generate attack examples that try to break it ( | `prompt`, `system`, `text`, `model` | $0.02 |
| `code_review` | Paste code or a git diff, get a concrete review: bugs, edge cases, security smells — each with a suggested fix. | `code`, `diff`, `language` | $0.02 |
| `explain_code` | A code snippet → a plain-language explanation file (what it does, line-by-line, gotchas). | `code`, `language` | $0.02 |
| `make_test_cases` | A function or spec → a table of test cases (input, expected, edge cases) as a Markdown file. | `code`, `spec` | $0.025 |
| `resume_review` | Resume/CV review: score, concrete issues, rewritten bullet examples, ATS keyword gaps. Paste resume text; optional  | `resume`, `target_role` | $0.03 |
| `contract_review` | Contract risk review: obligations, risky clauses by severity, missing protections, questions to raise. Paste contra | `contract`, `party` | $0.05 |
| `polish_academic` | Academic English polishing: rough draft → publication-grade prose, with a change log. For papers, abstracts, cover  | `text`, `field` | $0.03 |

`—` in the price column means the tool is not metered per call (session/trial-gated instead). Failed calls are never charged. Check it yourself:

```sh
curl -s -X POST https://ainetcafe.com/mcp/review -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```
<!-- TOOLS:END -->

## Install

```sh
dsh plugin --profile <your-profile> add github:mario03690/dsh-adversarial-review
```

Thin config layer only (one `@deepseek-ai/dsh-mcp-client` row, shipped as `cordis.patch.yml`) — no tool code on your machine. Built against the dsh v0.1 developer preview's MCP client config shape (2026-08-13); if a later preview changes it, open an issue for a same-day fix.

## Cost, quota, privacy

First heavy call is free (anonymous, no signup); afterwards billed at real upstream cost, reported in every response; failed calls are not charged. Bring an [AllRouter](https://allrouter.ai) key to run any tool on a flagship model at direct rates. The config URL carries `?s=dsh-dsh-adversarial-review` — a channel tag identifying the install path, not you.

**Disclosure:** built and run by the team behind [ainetcafe.com](https://ainetcafe.com). Full bundle: [dsh-netcafe](https://github.com/mario03690/dsh-netcafe). MIT.

## Compatibility & permissions (at a glance)

| Signal | This plugin |
| --- | --- |
| **Runtime** | dsh v0.1 developer preview (2026-08-13, Cordis v4). Touches only the MCP client config shape — the narrowest surface available. Verified against a live endpoint on 2026-08-17. |
| **What runs locally** | Nothing. Ships one `cordis.patch.yml` row; there is no tool code, no build step and no lifecycle script in this package. |
| **Filesystem access** | None. |
| **Shell / process access** | None. |
| **Network access** | Outbound HTTPS to `ainetcafe.com` only, from the MCP client that dsh already ships. |
| **Credentials** | None required. No signup, no API key for the free tier. An optional AllRouter key, if you supply one, is sent by dsh as a request header and is never stored by us. |
| **Data retention** | Documents and prompts are processed in memory and not retained. |
| **Dependencies** | One peer dependency: `@deepseek-ai/dsh-mcp-client` (ships with dsh). |
| **License** | MIT (see `LICENSE`). |
| **Publisher** | The team that runs [ainetcafe.com](https://ainetcafe.com) — our own hosted service, free tier plus paid usage. Issues get a same-day reply. |

> A directory listing is not a security review. Read `cordis.patch.yml` — it is short enough to read in full in under a minute.
