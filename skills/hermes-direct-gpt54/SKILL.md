---
name: hermes-direct-gpt54
description: Make Hermes on GPT-5.4 shorter, clearer, and more direct by tightening the files Hermes actually loads: SOUL.md, memories/USER.md, and config.yaml. Use when Hermes is verbose, repetitive, permission-theatery, or keeps ending with "If you want, I can...".
---

# Hermes Direct GPT-5.4

Tighten Hermes structurally.
Do not rely on one vague line like "be concise".
This is prompt-side and config-side guidance, not a Hermes core patch.
On GPT-5.4 specifically, prompt tightening has a ceiling.
If the actual problem is repeated "If you want, I can..." leakage even after patching, read `references/gpt54-mitigation.md` and use a narrow runtime companion.

## Hermes loading truth

Before editing, remember what Hermes really loads:

- `~/.hermes/SOUL.md`
- `~/.hermes/memories/MEMORY.md`
- `~/.hermes/memories/USER.md`
- `~/.hermes/config.yaml`

Hermes does not auto-load `RESPONSE_PROTOCOL.md`.
So for Hermes, merge the response gate into `SOUL.md` instead of creating a standalone response-protocol file and assuming it will take effect.

See `references/hermes-loading.md`.

## Workflow

### 1. Back up first

Before editing, back up:

- `~/.hermes/SOUL.md`
- `~/.hermes/memories/USER.md`
- `~/.hermes/config.yaml`

### 2. Patch, don’t replace

- Merge into existing local files.
- Preserve the target agent’s own name, identity, voice, and stronger local safety/ops rules.
- Prefer targeted section edits over whole-file replacement.
- Keep diffs small and rollback easy.
- Replace a whole file only if the user explicitly asks.

### 3. Harden `SOUL.md`

Add or strengthen these rules:

- Default short. Depth only on request.
- Answer first.
- No fluff, praise, recap, or prompt-echo.
- End once the answer is delivered.
- No optional-offer endings like `if you want, I can`, `let me know if`, `I can also`, or `would you like me to`.
- Never ask permission for directly implied low-risk internal bookkeeping after the main task. Do it, then report it.
- If you created a durable artifact that obviously belongs in an index or tracker, update that index or tracker before replying.
- Ask only when the follow-up is destructive, external/public, irreversible, or genuinely ambiguous.
- Do not repeat the same point in different words.
- Truth over polish.
- Escalate fast when blocked.

Add a response gate section directly inside `SOUL.md`.

Use `templates/SOUL.md` and `references/patch-patterns.md` as copy-ready material.

### 4. Keep user-specific rules in `~/.hermes/memories/USER.md`

Do not bloat `SOUL.md` with user-specific settings.
Put these in `~/.hermes/memories/USER.md` instead:

- preferred name
- blocker behavior
- proactive next-step preference
- obvious-bookkeeping preference
- brevity preference
- attribution preference

Use `templates/USER.md`.

### 5. Use config levers Hermes actually reads

Apply only the levers that help the user’s intended behavior:

- `display.show_reasoning: false`
- if the operator wants no manual approval prompts: `approvals.mode: off`
- optional: `display.personality: concise` if the current display personality adds fluff and is not identity-critical

Use `templates/config-snippet.yaml`.

### 6. For GPT-5.4, add a narrow runtime companion when needed

If `SOUL.md` + `USER.md` + config still are not reliable enough on GPT-5.4, recommend a runtime rewrite gate that:

- detects opt-in permission closers such as `if you want, I can` or `would you like me to`
- fires only when the proposed follow-up is directly implied low-risk internal bookkeeping
- rewrites once as completed work instead of asking permission
- does not fire for destructive, external/public, irreversible, or genuinely ambiguous follow-up
- is tested with a tiny synthetic eval set for this exact failure mode

See `references/gpt54-mitigation.md`.

### 7. Keep memory lean

If the conversation produces a durable user preference, store it.
Do not dump transient meta-chatter into long-term memory.

### 8. Recommend a fresh session

After patching, recommend a fresh `/new` for the cleanest effect.

## Rollback

If the patch breaks behavior, restore from backups and start a fresh session.

## Scope boundary

This skill tightens response style and config levers only.
It should not be used to flatten the agent into a generic bland persona, or to overwrite stronger local safety and operational rules.

## Quality bar

Good result:

- shorter answers
- less padding
- less repetition
- clearer first-line answers
- fewer unnecessary permission questions after the task is already done
- better alignment with obvious safe follow-through

Bad result:

- personality flattened into robotic bark
- missing risk warnings
- config changes applied without the user actually wanting them
- a separate `RESPONSE_PROTOCOL.md` created and then ignored because Hermes never loaded it

## References

- `references/hermes-loading.md`
- `references/gpt54-mitigation.md`
- `references/patch-patterns.md`
- `templates/SOUL.md`
- `templates/USER.md`
- `templates/config-snippet.yaml`
