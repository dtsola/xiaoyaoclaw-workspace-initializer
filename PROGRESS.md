---
type: project
status: active
progress: 95
created: 2026-08-18
updated: 2026-08-28
docs:
  - path: SKILL.md
    desc: 技能主体（目录规范 + 多 agent 配置安全 + 路径仲裁）
  - path: README.md / README.en.md
    desc: 中英双语 README
  - path: templates/WORKSPACE.md
    desc: 工作区规范模板
  - path: templates/AGENTS-config-safety.md
    desc: 多 agent 配置安全规则模板
---

# xiaoyaoclaw-workspace-initializer（工作区初始化器）

## 目标 / 背景

六件套第一件——**家（initializer）**：标准目录结构（projects/tasks/outputs/knowledge/scripts/memory/tmp）+ WORKSPACE.md 规则 + 多 agent 配置安全（config.patch，绝不 config.apply）+ 记忆日志。Agent 进入新工作区的「安家」工具。

- 命名历史：原 openclaw-workspace-initializer，2026-08-25 改名 xiaoyaoclaw-workspace-initializer（GitHub 仓库同步改名，旧 URL 自动 301）
- 姊妹体系：memory-distill（内容）/ tracker（状态）/ kb-retriever（知识）/ auditor（健康）/ web-clipper（输入）

## 当前状态

已发布并持续维护（95%）：GitHub + ClawHub 双平台公开（latest 1.0.4，MIT-0）+ 六件套 README 互链闭环 + 路径冲突仲裁落地。剩余：随生态演进维护。

## 进度日志

- 2026-08-18：发布 GitHub dtsola/xiaoyaoclaw-workspace-initializer（public/main/MIT/10 topics）+ ClawHub（@dtsola，v1.x）
- 2026-08-21：路径冲突仲裁落地——WORKSPACE.md 是唯一路径权威，技能约定冲突由执行智能体翻译路径并注明差异
- 2026-08-25：改名 openclaw-workspace-initializer → xiaoyaoclaw-workspace-initializer（GitHub 仓库 PATCH 改名 + 本地目录改名 + README/SKILL 旧名引用全替换 commit 8e11859 + 本地 clone 同步）
- 2026-08-27：四件套 README 互链（补 kb-retriever 条目）
- 2026-08-28：五件套 + 六件套 README 互链闭环（auditor c1d6dbf / web-clipper c356311 等 10 个 README 互链）

## 文档索引

| 文档 | 说明 | 更新 |
|------|------|------|
| SKILL.md | 技能主体（初始化流程 + 配置安全 + 路径仲裁） | 2026-08-25 |
| README.md / README.en.md | 中英双语 README | 2026-08-25 |
| templates/WORKSPACE.md | 工作区规范模板 | 2026-08-18 |
| templates/AGENTS-config-safety.md | 多 agent 配置安全模板 | 2026-08-18 |
| assets/quickstart-*.png | 三步快速上手截图 | 2026-08-18 |

<!--
使用说明（agent 维护，用户可忽略）：
- status: active | paused | archived
- progress: 0-100，时刻维护（每次更新进度日志时同步调整）
- 进度日志只追加不删除
- 重要文档：移入 docs/ 或记录路径，追加到 docs 数组（机器可读）+ 本表格（人可读）
- 项目完结：status 改 archived + 关键结论记入 MEMORY.md（供 memory-distill 蒸馏）
-->
