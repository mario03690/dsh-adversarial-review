# dsh-adversarial-review

Adversarial review for code, MCP configs and system prompts — every finding carries a repro path and confidence; unreproducible ones are marked suspected, never confirmed.

## What & why

Not a scanner: it reviews code/config/prompts YOU provide, it does not attack third-party services. `adversarial_review` probes named surfaces (injection, trust boundaries, secret handling, side effects, idempotency, swallowed errors) — each finding with a repro path and high/medium/low confidence, and anything it cannot reproduce is downgraded to *suspected*, not presented as confirmed (the same honesty contract as our tables' arithmetic self-check). `mcp_config_audit` checks agent/MCP configs for over-broad grants, embedded secrets, unannotated destructive tools. `red_team_prompt` attacks your own system prompt and hands back hardening. Pass `model` to run on a flagship (Claude/Kimi K3/GPT via AllRouter).

Start with `what_can_you_do` — describe your task in any language, get the exact tool and a ready-to-run call.

## Install

```sh
dsh plugin --profile <your-profile> add github:mario03690/dsh-adversarial-review
```

Thin config layer only (one `@deepseek-ai/dsh-mcp-client` row, shipped as `cordis.patch.yml`) — no tool code on your machine. Built against the dsh v0.1 developer preview's MCP client config shape (2026-08-13); if a later preview changes it, open an issue for a same-day fix.

## Cost, quota, privacy

First heavy call is free (anonymous, no signup); afterwards billed at real upstream cost, reported in every response; failed calls are not charged. Bring an [AllRouter](https://allrouter.ai) key to run any tool on a flagship model at direct rates. The config URL carries `?s=dsh-dsh-adversarial-review` — a channel tag identifying the install path, not you.

**Disclosure:** built and run by the team behind [ainetcafe.com](https://ainetcafe.com). Full bundle: [dsh-netcafe](https://github.com/mario03690/dsh-netcafe). MIT.
