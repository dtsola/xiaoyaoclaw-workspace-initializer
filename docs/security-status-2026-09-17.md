# ClawHub 安全检查核查与修复（2026-09-17）

> 执行人：天桐｜指令：指挥官「处理 Openclaw Workspace Initializer 的问题」
> 命令：`clawhub skill verify xiaoyaoclaw-workspace-initializer`（对象 v1.0.5）

---

## 1. 结论

`ok:false` / `decision:fail` / 原因码 `security.status_not_clean`；`security.status = suspicious`（confidence high）。
**共 5 条命中**：aig 2（T01/T02，均为 **error / High**）+ skillspector 3（SQP-1 + SQP-2 ×2，MEDIUM）。

LLM 判词：*"This skill is a workspace initializer, but it can persistently change future agent behavior and override other skills' file-placement rules."*

> 特点：这个技能的**核心功能本身就是**「建目录 + 写持久规范 + 写 AGENTS.md」，所以两条 aig 命中打在功能线上 —— **不能靠删功能过关，只能加闸门与边界**。

---

## 2. 命中与修复对照

| 命中 | 位置 | 问题 | 修复 |
|---|---|---|---|
| **aig T01** error（Skill Instruction Hijacking） | `SKILL.md:60-80`、`templates/WORKSPACE.md:27-36` | 「路径冲突仲裁」写的是**一律以本文件为准**（强行压过其他技能的约定） | 改写为「**文件放置约定**」并限定作用域：只管**本工作区产出的文件放哪**；**技能自身的安全规则、许可条款与既定流程优先**，不得要求技能绕过；技能必须在工作区外写入时**以技能为准**；换位置解决不了的冲突**停下来问用户** |
| **aig T02** error（Agent Memory Poisoning） | `SKILL.md:82-107`、`templates/WORKSPACE.md`、`templates/AGENTS-config-safety.md` | 自动把规则写进 `AGENTS.md` 并追加 memory 日志 → 等于静默改变**未来每次会话**的行为 | Step 4.5 改为**默认只提议**：先给**确切 diff** + 明示「这会让未来每次会话启动都先读 WORKSPACE.md」，**得到明确同意才写**；不同意就跳过并在汇报里注明「未改动（待你确认）」；Step 5 的日志写入纳入同一轮批准；明确 `AGENTS.md`/`WORKSPACE.md`/`SOUL.md`/`USER.md` = **用户资产** |
| **SQP-1** MEDIUM | `SKILL.md:4`（description 激活面过宽） | 可能在非空/已管理的仓库里擅自做结构化改动 | description + 新增「**激活边界**」章节：仅在①用户明确要求初始化/修复 ②发现确缺标准目录**且**当轮任务本就要落文件时激活；已有工程结构、已存在别的工作区规范、只是提问讨论、或**工作区已合规（什么都不做）** 时**不激活** |
| **SQP-2** MEDIUM | `SKILL.md:48`（建目录/写 WORKSPACE.md 无确认） | 静默改动仓库布局与策略文件 | 新增 **Step 0 闸门（默认 dry-run）**：先出差异清单（缺失什么 / 将创建什么 / **不会触碰什么** / 需单独批准什么）→ 等用户确认 → 才进入执行；Step 2/3 标题与正文都写明「用户确认后执行」「已存在则不覆盖」 |
| **SQP-2** MEDIUM | `SKILL.md:84`（把规则持久化进 AGENTS.md / 追加日志） | 未授权的持久化改动 | 同 aig T02 的批准闸门；并补「不装常驻机制：不建 cron、不起守护进程、不写启动脚本、不写跨会话状态文件」 |

**连带一致化**：README 中英、`templates/WORKSPACE.md` 抬头与第 10 条、`templates/AGENTS-config-safety.md` 抬头，全部改为与新口径一致（避免文档之间自相矛盾又被复核揪出来）。

**包内容卫生**：新增 `.clawhubignore`，把 `PROGRESS.md`（仓库侧过程记录）排除出发布包 —— 沿用 SEO 那轮学到的教训（扫描器会把文档当技能内容读）。

---

## 3. 验证

- 旧口径残留扫描：README 中英 / SKILL.md / 两个模板 **均为「无」**（`workspace rules win`、`一律以工作区规范为准` 等已清除）
- 新口径到位：SKILL.md 与模板均含「确认闸门 / 不覆盖 / 用户资产 / 技能规则优先」
- 结构校验：SKILL.md frontmatter 完整、章节顺序 Step 0→1→2→3→4→4.5→5 连续、示例章节同步更新
- 发布包预览（复刻 CLI `listSkillFiles` 逻辑）：**11 个文件**，`PROGRESS.md` 已排除
- 本技能无脚本、无网络访问、无外部依赖 → 不涉及运行时回归

---

## 4. 待办

- [ ] 发 **v1.0.6** → 等扫描 → `clawhub skill verify` 复扫核对 5 条清零
- [ ] ⚠️ **本地已安装副本（`state/skills/xiaoyaoclaw-workspace-initializer/`，2026-09-01 版）仍是旧口径** → 是否同步更新，待指挥官决定
- [ ] ⚠️ **本工作区自己的 `WORKSPACE.md`**（含旧口径第 10 条）是否按新模板同步，待指挥官决定（属用户资产，不擅自改）
- [ ] GitHub 推送（代理 22307 未监听 + 直连超时）

## 5. 原始证据

- `docs/evidence/verify-v1.0.5-2026-09-17.json`
