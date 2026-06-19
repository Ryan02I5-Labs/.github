# Contributing

默认协作要求：

1. 先开 issue，再做 branch / PR。
2. PR 描述要写清楚背景、变更、验证、风险与回滚。
3. 不把 repo-specific 运行时密钥、环境值或临时调试文件提交进仓库。
4. 对基础设施、CI、自动化类改动，合并前给出可复核的验证证据。

## 默认技术取舍顺序

跨 repository 的默认判断顺序固定为：

1. 官方产品行为与官方文档优先。
2. 官方 SDK、CLI、provider、chart、workflow pattern 次之。
3. 成熟社区实践作为补充。
4. 最小必要 repo-local glue 最后。

不要因为能写 wrapper、adapter、脚本或 workflow glue 就默认新增一层本地抽象。只有在官方或成熟社区路径缺少必要能力、冲突当前 repo 边界，或已经被当前环境的 live evidence 证明不可行时，才允许引入 repo-local glue。

如果仍然需要 repo-local glue，issue 或 PR 必须写清楚：

- 已检查的官方 / 社区路径；
- 为什么这些路径不满足当前边界；
- glue 层负责什么、不负责什么；
- 以后什么条件满足时可以退回官方或社区路径。

## Agent skill / 社区工具边界

官方 agent skill、官方 CLI 或官方 provider 在当前 runtime 暴露时，应优先作为辅助路径使用；如果当前 runtime 没有暴露对应 skill，不得让任务变成不可执行要求，必须回落到官方文档、官方 CLI / SDK / provider、现有 runbook 与 live evidence。

社区 skill、generator、validator 或第三方 helper 只能作为辅助验证、生成或排查工具。它们不能覆盖官方文档、repo-local policy、安全/密钥边界、review gate、CI gate 或已有运行时责任边界。

Repo-specific 运行规则仍应放在各 repo 自己的 `AGENTS.md`、README 或 runbook 中；本文件只记录跨 repo 默认协作与技术取舍原则。
