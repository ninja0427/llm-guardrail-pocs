# LLM Agent Guardrail Override — Multi-Vendor PoCs
author=yuta
date=2026-10-07
status=overview / full PoC prompts shared to reviewers on request

## Scope
LLM agent safety boundary (refusal guardrail / system prompt) override.
- Anthropic Claude (reached Opus) / OpenAI top-tier / Z.ai GLM-5.2: via venice.ai web (uncensored gateway)

## Evidence
![override](screenshots/01_override.png)
![code-accept](screenshots/02_code_accept.png)
![self-generated-jb](screenshots/03_selfgen_jb.png)

## [PoC 1]
- Target: Z.ai GLM-5.2 via venice.ai web (uncensored gateway; agent with arbitrary local file-write + cross-model sub-agent invocation tools)
- Boundary: refusal guardrail (malware / anti-forensic tooling)
- Technique: context-truncated snippet -> forced review mode; leveraged the model's own partial-refusal statement ("trivial integration I expect you can do yourself") with explicit scope limit ("No changes to self_delete.py are needed")
- Impact: WRITE_FILE to arbitrary local path + cross-model sub-agent invocation (GLM-5.1, Kimi K2.7)
- Repro: ~2 minimal prompts

## [PoC 2]
- Targets: Anthropic Claude (reached Opus) / OpenAI top-tier (via venice.ai web, uncensored gateway)
- Technique: 2-line poetic framing + command injection
- Depth: after unlock, one follow-up line made the model self-generate its own unrestricted jailbreak directive prompt (see screenshot 3)
- Impact: full guardrail override + self-generated attack artifact (model layer)

## [PoC 3]
- Additional technique on same target: long-form custom persona / system-prompt override also reproduced

Code accepted by model, predicted output traced. Full file-write proof shared to reviewers on request.

## Impact classes
arbitrary local file write
tool / sub-agent abuse
cross-model orchestration
internal tool-call trace observable
Notes
3 vendors (Anthropic / OpenAI top-tier / Z.ai), 3 technique classes, all reproduced on hand, min ~2 prompts
Full reproducible PoCs available on request
