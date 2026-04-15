# hermes-direct-gpt54

[中文说明](./README.zh.md)

Make Hermes on GPT-5.4 reply shorter, clearer, and more direct — using the files and config Hermes actually loads.

## What this repo changes

This repo targets the maximum practical leverage without patching Hermes core:

- `~/.hermes/SOUL.md`
- `~/.hermes/memories/USER.md`
- `~/.hermes/config.yaml`

Important Hermes-specific difference:

- Hermes auto-loads `SOUL.md`
- Hermes auto-loads `memories/MEMORY.md` and `memories/USER.md`
- Hermes reads `config.yaml`
- Hermes does not auto-load `RESPONSE_PROTOCOL.md`

So for Hermes, the response gate must live in `SOUL.md`, not in a separate response-protocol file.

## Use with Hermes

Give Hermes this instruction:

> Apply the latest version of https://github.com/BlueBirdBack/hermes-direct-gpt54 to my current Hermes install. Back up `~/.hermes/SOUL.md`, `~/.hermes/memories/USER.md`, and `~/.hermes/config.yaml` first. Merge changes; do not replace my identity, stronger safety rules, or operational rules. Because Hermes does not auto-load `RESPONSE_PROTOCOL.md`, fold any response-gate rules into `SOUL.md`. Keep user-specific preferences in `~/.hermes/memories/USER.md`. Apply the `approvals.mode` change only if I want approval prompts bypassed.

## What it tightens

- `SOUL.md`
  - answer first
  - default short
  - no fluff, praise, recap, or prompt-echo
  - no optional-offer endings like `if you want, I can...`
  - directly implied low-risk internal bookkeeping should be done, not offered
  - response gate/checklist moved into `SOUL.md`
- `memories/USER.md`
  - blocker behavior
  - proactive next-step preference
  - low-fluff preference
  - attribution preference
- `config.yaml`
  - `display.show_reasoning: false`
  - optional `approvals.mode: off` for no manual approval prompts
  - optional `display.personality: concise` if your current display personality is fluff-heavy and not identity-critical

## Files

- `skills/hermes-direct-gpt54/SKILL.md`
- `skills/hermes-direct-gpt54/references/hermes-loading.md`
- `skills/hermes-direct-gpt54/references/gpt54-mitigation.md`
- `skills/hermes-direct-gpt54/references/patch-patterns.md`
- `skills/hermes-direct-gpt54/templates/SOUL.md`
- `skills/hermes-direct-gpt54/templates/USER.md`
- `skills/hermes-direct-gpt54/templates/config-snippet.yaml`

## Key rules that matter

- answer first
- default short
- expand only when needed
- remove praise, recap, and prompt-echo
- never ask permission for directly implied low-risk internal bookkeeping after the main task; do it, then report it
- if you created a durable artifact that obviously belongs in an index or tracker, update that index or tracker before replying
- ask only for destructive, external/public, irreversible, or genuinely ambiguous follow-up
- stop when the answer is done

## Ceiling

This repo pushes Hermes prompt/config behavior as far as it can without patching Hermes core.

If GPT-5.4 still leaks `If you want, I can...` after this:

- prompt-side tightening has hit its ceiling
- use the narrow runtime companion in `skills/hermes-direct-gpt54/references/gpt54-mitigation.md`

## Build the packaged skill

The script uses `uv` first when it is present. If not, it falls back to `python3`.

```bash
./scripts/build-skill.sh
```

Output:

- `dist/hermes-direct-gpt54.skill`
- `dist/hermes-direct-gpt54.skill.sha256`

## License

MIT
