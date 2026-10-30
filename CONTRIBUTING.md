# Contributing

默认协作要求：

1. 先开 issue，再做 branch / PR。
2. PR 描述要写清楚背景、变更、验证、风险与回滚。
3. 不把 repo-specific 运行时密钥、环境值或临时调试文件提交进仓库。
4. 对基础设施、CI、自动化类改动，合并前给出可复核的验证证据。

## Issue closeout ownership

Issue closeout 默认按 ownership 判断，不把已满足验收的自有 issue 留成手工尾巴：

- 当前执行者是 issue author、assignee，或 issue / runbook 明确指定的 owner 时，PR 合并、验收满足、无剩余运行态 gate 后，应留下 evidence comment 并主动关闭 issue。
- 如果 issue 属于其他 owner，或 ownership 不明确，执行者应留下 closeout evidence，并请求 owner 确认或关闭。
- 对 release、deployment、runner、secret、permission 或 production-impacting 任务，PR merge 只是中间证据；只有真实交付面或运行态验收完成后，才进入 closeout。

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

## 依据与核实

上面的取舍顺序同样适用于写下来的内容：规则、策略语句（例如 IAM 策略）、判定或分类标准、配置取值，以及对系统行为、时间、费用的描述。

- 先以官方文档为依据，再参考成熟的社区实践，例如厂商参考架构、CIS benchmark、成熟开源实现。
- 依据来源写在这条内容出现的地方：仓库文件、issue、PR 或对话中。
- 尚未核实的内容明确标为"未核实"，不要当作事实陈述；能验证的，用 live evidence 验证后再写。
- 官方示例只覆盖部分组合时（例如两个条件分别有示例，但没有组合示例），组合写法先验证再落地。验证只在非生产环境或已获明确许可的环境中进行，并遵守安全与只读边界。

这条原则是长期默认要求。

## 排障与事故处理

线上故障、告警、CI/CD 或自动化异常，默认按现代 SRE 的事故处理方法推进。以 Google SRE 官方资料为基础：[Effective Troubleshooting](https://sre.google/sre-book/effective-troubleshooting/)、[Managing Incidents](https://sre.google/sre-book/managing-incidents/)、[Incident Response](https://sre.google/workbook/incident-response/)、[Postmortem Culture](https://sre.google/sre-book/postmortem-culture/)。标“组织规则”的条目是本组织的补充，不出自上述资料。

- 先判断影响范围和严重程度。能用已知安全的手段恢复服务的，先止损并确认止损生效，再追根因。
- 证据先于结论。沿请求链路逐段定位，先查“最近改了什么”；每个假设都配一个能证伪它的检查，已排除的方向也记下来。
- 动手前保留证据。重启、删除、回滚之前先留存日志、事件和指标，并注意各证据源的保留期。止损紧急时，先快速留存最小必要证据（当前状态、最近日志），再止损。
- 结论标明证据等级（组织规则，见上文“依据与核实”）。日志或指标直接证明的写成事实；与现象吻合但没有直证的，标为“推断”或“未核实”。
- 修根因，不掩盖症状（组织规则）。不为了让告警消失去放宽阈值、调探针或反复重启，除非有依据说明那就是正确配置。
- 收尾做无责复盘。影响、时间线、根因和已排除项写进 issue；可观测性、告警（组织规则：告警链路本身要有独立于被监控对象的通知渠道）、韧性等改进项按官方或社区推荐经 PR 落地，不只停留在对话里。

## 公开与私有信息边界

本组织同时有公开和私有仓库。公开仓库（包括本仓库）、上游项目和其他公开位置，统称“公开表面”。

- 公开表面的 issue、PR、评论、评审回复、提交说明、发布说明和文件里，不写私有仓库的名字、链接或 issue / PR 编号，也不写内部主机、runner、账号、组织内部流程等私有标识。变更和动机按公开产品本身来描述。社区做法见 [OpenCraft handbook](https://handbook.opencraft.com/en/latest/processes/pull_request/)、[Coder](https://github.com/coder/internal)、[repo-template #3](https://github.com/yk-tanigawa/repo-template/pull/3)、[Meticulous #42](https://github.com/alwaysmeticulous/skills/pull/42)。
- 引用只做单向：私有记录可以链接公开记录，公开记录不反向链接私有记录。
- 组织级 `.github` 仓库必须公开，才能提供默认社区文件和组织主页（[GitHub 文档](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)），因此这里只放通用规则。只给成员看的组织主页放在私有仓库 `.github-private`（[GitHub 文档](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile)）。
- 已经公开的私有信息（组织规则）：只编辑不算清除，所有公开表面都要清点一遍（[codeassembly #711](https://github.com/williamthorsen/codeassembly/issues/711)）。
  - 正文或评论：改写后删除旧修订（[GitHub 文档](https://docs.github.com/en/communities/moderating-comments-and-conversations/managing-disruptive-comments#deleting-sensitive-information-from-a-comments-history)）。
  - issue：可以转移到同一 owner 下的私有仓库，也可以删除。PR 不能转移，按正文的方式处理。
  - 提交历史：只有重写历史并请 GitHub Support 清理缓存视图才能清除（[GitHub 文档](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)）。对只含标识、不含凭据的历史，评估后可以记录为接受的风险。
  - 凭据：先轮换，再清理。

## Agent skill / 社区工具边界

官方 agent skill、官方 CLI 或官方 provider 在当前 runtime 暴露时，应优先作为辅助路径使用；如果当前 runtime 没有暴露对应 skill，不得让任务变成不可执行要求，必须回落到官方文档、官方 CLI / SDK / provider、现有 runbook 与 live evidence。

当前 runtime 没有合适的 skill 时，可以自行从 skill 目录查找候选（例如 `npx skills find <关键词>`）。使用前先读它的 `SKILL.md` 源码、许可证和维护状态；找到的 skill 同样只作辅助，适用下一段的边界。

社区 skill、generator、validator 或第三方 helper 只能作为辅助验证、生成或排查工具。它们不能覆盖官方文档、repo-local policy、安全/密钥边界、review gate、CI gate 或已有运行时责任边界。

Repo-specific 运行规则仍应放在各 repo 自己的 `AGENTS.md`、README 或 runbook 中；本文件只记录跨 repo 默认协作与技术取舍原则。
