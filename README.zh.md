# hermes-direct-gpt54

[English](./README.md)

让 Hermes 里的 GPT-5.4 回答更短、更清楚、更直接，而且尽量用 Hermes 真正会加载的入口去生效。

## 这个仓库改什么

这个仓库瞄准的是：在不修改 Hermes 内核的前提下，尽量把效果做到最大。

- `~/.hermes/SOUL.md`
- `~/.hermes/memories/USER.md`
- `~/.hermes/config.yaml`

Hermes 和 OpenClaw 有个关键区别：

- Hermes 会自动加载 `SOUL.md`
- Hermes 会自动加载 `memories/MEMORY.md` 和 `memories/USER.md`
- Hermes 会读取 `config.yaml`
- Hermes 不会自动加载 `RESPONSE_PROTOCOL.md`

所以在 Hermes 里，回复闸门要并进 `SOUL.md`，不能指望单独放一个 `RESPONSE_PROTOCOL.md` 就自动生效。

## 给 Hermes 用

直接对 Hermes 说：

> 把 https://github.com/BlueBirdBack/hermes-direct-gpt54 的最新版本应用到我当前的 Hermes 安装里。先备份 `~/.hermes/SOUL.md`、`~/.hermes/memories/USER.md` 和 `~/.hermes/config.yaml`。用合并方式修改，不要覆盖我现有的身份设定、更强的安全规则或运维规则。因为 Hermes 不会自动加载 `RESPONSE_PROTOCOL.md`，所以把任何回复闸门规则折叠进 `SOUL.md`。用户偏好放进 `~/.hermes/memories/USER.md`。只有当我明确想跳过审批提示时，才应用 `approvals.mode` 的改动。

## 它会收紧哪些点

- `SOUL.md`
  - 先给答案
  - 默认简短
  - 不要废话、夸人、复述、提示词回声
  - 不要用 `if you want, I can...` 这种可选收尾
  - 主任务做完后，凡是直接顺带就该做的低风险内部收尾，直接做完再汇报
  - 把回复前检查表直接放进 `SOUL.md`
- `memories/USER.md`
  - 卡住就立刻说
  - 下一步如果明显且安全，直接做
  - 低废话偏好
  - 署名 / 归因偏好
- `config.yaml`
  - `display.show_reasoning: false`
  - 可选 `approvals.mode: off`，用于跳过手动审批提示
  - 可选 `display.personality: concise`，前提是你当前的 display personality 本身不是重要身份风格

## 目录

- `skills/hermes-direct-gpt54/SKILL.md`
- `skills/hermes-direct-gpt54/references/hermes-loading.md`
- `skills/hermes-direct-gpt54/references/gpt54-mitigation.md`
- `skills/hermes-direct-gpt54/references/patch-patterns.md`
- `skills/hermes-direct-gpt54/templates/SOUL.md`
- `skills/hermes-direct-gpt54/templates/USER.md`
- `skills/hermes-direct-gpt54/templates/config-snippet.yaml`

## 真正关键的规则

- 先给答案
- 默认简短
- 只有确实需要时才展开
- 去掉夸奖、总结复述、提示词回声
- 主任务完成后，不要再为顺手就该做的低风险内部收尾额外征求许可
- 如果刚创建了一个明显该登记到索引或追踪清单里的持久产物，先补登记再回复
- 只有后续动作有破坏性、对外 / 公开、不可逆，或确实有歧义时才提问
- 事情做完就停

## 能力上限

这个仓库已经把 Hermes 在提示层 + 配置层能用的杠杆基本都用了，但它不改 Hermes 内核。

如果用了以后，GPT-5.4 还是会冒出 `If you want, I can...`：

- 说明提示层已经碰到上限
- 下一步该看 `skills/hermes-direct-gpt54/references/gpt54-mitigation.md` 里的窄运行时重写方案

## 构建 skill 包

这个脚本会优先用 `uv`。如果没有，再退回到 `python3`。

```bash
./scripts/build-skill.sh
```

输出：

- `dist/hermes-direct-gpt54.skill`
- `dist/hermes-direct-gpt54.skill.sha256`

## 许可证

MIT
