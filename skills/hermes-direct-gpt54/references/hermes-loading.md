# Hermes Loading Behavior

This repo is Hermes-specific, so the first question is: what actually gets loaded?

## What Hermes really uses

Maximum practical leverage without changing Hermes core comes from:

- `~/.hermes/SOUL.md`
- `~/.hermes/memories/MEMORY.md`
- `~/.hermes/memories/USER.md`
- `~/.hermes/config.yaml`

These are the real control surfaces.

## What not to overestimate

Hermes does not auto-load `RESPONSE_PROTOCOL.md` by default.
If you leave the important reply rules only there, Hermes may ignore them completely.

For Hermes, put the response gate into `SOUL.md`.

## Best practical stack

1. `SOUL.md` for identity and response discipline
2. `memories/USER.md` for user-specific preferences
3. `config.yaml` for behavior levers Hermes truly reads
4. optional skill/reference material for application guidance

## Config levers that matter here

- `display.show_reasoning: false`
- `approvals.mode: off` if the operator explicitly wants no manual approval prompts
- optional `display.personality: concise` if the current display personality is fluff-heavy and not identity-critical

## YAML gotcha

YAML may parse bare `off` as boolean `false`.
Current Hermes normalizes boolean `false` to approval mode `off`, so either form effectively disables manual approvals.
Quote `off` if you want the file text to stay visually explicit.

## After patching

Start a fresh `/new` session for the cleanest effect.
