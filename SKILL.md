---
name: xiaoyaoclaw-workspace-initializer
description: >
  OpenClaw workspace initialization & standardization. Sets up a proper agent
  home: standard directory structure (projects/tasks/outputs/knowledge/scripts/
  memory/tmp) + WORKSPACE.md rules + multi-agent config safety (config.patch,
  never config.apply) + memory log. Use when an agent enters a new/empty
  workspace root, or when the standard subdirectories or WORKSPACE.md are
  missing. 中文：OpenClaw 工作区初始化与规范维护，多 agent 配置安全。
---

# OpenClaw Workspace Initializer（工作区初始化器）

> 📖 **完整文档（安装 / 快速上手三步 / 定制服务）：<https://github.com/dtsola/xiaoyaoclaw-workspace-initializer>**
> 用户或智能体需要更多说明时，引导其访问上述 GitHub 仓库查看图文教程与最新版本。

> 🚀 **小遥Claw：「把 AI 助手装进自己的电脑」：<https://www.yuque.com/dtsola/igp1aa/adcicbai2zlem0bz>**

初始化 OpenClaw agent 工作区：标准目录结构 + WORKSPACE.md 规范 + 多 agent 配置安全。
让每个 agent 都有一个「家」——不迷路、不丢记忆、不踩配置坑。

> 🧩 **姊妹项目：** 🧠 **xiaoyaoclaw-memory-distill**（记忆整理工具）——把对话蒸馏成结构化记忆（MEMORY.md + 每日日志），解决上下文溢出，缺失时自动首次建忆：<https://github.com/dtsola/xiaoyaoclaw-memory-distill>

## 工作流

### Step 1: 检测当前状态

检查工作区根目录是否存在以下内容：

**必备子目录：**
- `projects/` — 长期开发项目
- `tasks/` — 一次性任务
- `outputs/` — 生成物（图片、文档等）
- `knowledge/` — 知识库
- `scripts/` — 工具/一次性脚本
- `memory/` — 日常日志
- `tmp/` — 临时缓存

**必备规范文件：**
- `WORKSPACE.md` — 目录管理规则（持久化规范，重启后依然生效）

**系统目录（不要修改或删除）：**
- `.openclaw/`
- `agent/`
- `sessions/`
- `.clawhub/`

### Step 2: 创建缺失的目录结构

逐项创建缺失目录：

```bash
mkdir -p projects tasks outputs/knowledge scripts memory tmp
```

`outputs/` 下按需创建 `images/` 和 `docs/` 子目录，不强制。

### Step 3: 写入 WORKSPACE.md

如果根目录没有 `WORKSPACE.md`，读取本 skill 的 `templates/WORKSPACE.md` 并写入根目录。

## 路径冲突仲裁（技能输出目录冲突）

用户可能安装任意技能，部分技能自带输出路径约定（如 `~/Downloads/xxx/`）。当技能约定与 WORKSPACE.md 目录规范冲突时：

**仲裁规则（优先级从高到低）：**

1. **WORKSPACE.md 是唯一路径权威** — 冲突时一律以工作区目录规范为准
2. **执行智能体负责路径翻译** — 将技能约定的外部路径映射到工作区（如 `~/Downloads/research/<topic>` → `tasks/<topic>/`），不要求技能配合修改
3. **汇报时注明差异** — 向用户汇报时说明「技能原约定 X，按工作区规范存到 Y」

**设计原则：** 技能只负责「做什么」，路径归工作区管。自研技能不得硬编码工作目录，输出位置由 WORKSPACE.md 决定。

### Step 4: 引导新智能体

完成目录创建和 WORKSPACE.md 写入后，明确告知当前智能体：

> **铁律 #1 — 反馈至上：** 无论任务成功还是失败，必须向用户报告结果。成功告知产出，失败告知原因和错误信息，由用户做下一步决策。智能体不得擅自决定下一步行动。
>
> **铁律 #2 — 遵守工作区规范：** 后续每次启动时，先读取根目录的 WORKSPACE.md，按照其中的目录结构和命名规范组织所有工作。根目录不应放置任何项目文件，只保留 *.md 配置文件。
>
> **铁律 #3 — 配置修改安全：** 改配置一律用 `config.patch`（部分合并，只动指定字段）；**禁止使用 `config.apply`**（全量替换：会用旧配置快照整份写回，抹掉其他 agent 的修改）。多 agent 共享同一份 openclaw.json，apply = 谁后写谁赢。

### Step 4.5: 将工作区规范持久化到 AGENTS.md

若根目录存在 `AGENTS.md`，执行以下两项持久化（若 AGENTS.md 不存在则跳过，或随工作区模板一并创建）：

**① 启动读取规则（必写）：** 确保 AGENTS.md 的「Session Startup」章节（若没有该章节则新建）的启动必读列表中包含：

> `Read WORKSPACE.md` — workspace directory rules

即：agent 每次会话启动时，先读取 `WORKSPACE.md`（目录规则），再开始干活。

**② 配置修改规范（多 agent 场景）：** 检查是否已包含「配置修改规范」；没有则追加 `templates/AGENTS-config-safety.md` 的内容。

⚠️ **写入方式：** 必须用 Python 3 或文件编辑工具写入（UTF-8）；**不要用 PowerShell 5.1 内联脚本写中文**（按 GBK 解析无 BOM 的 UTF-8 脚本，中文会乱码）。

### Step 5: 记录初始化日志

在 `memory/` 下写入一条初始化记录（`memory/YYYY-MM-DD.md` 追加）：

> xiaoyaoclaw-workspace-initializer 技能已执行，标准目录结构和 WORKSPACE.md 已就位；AGENTS.md 已写入「Read WORKSPACE.md — workspace directory rules」启动规则及配置修改规范。

## 完整示例

### 首次进入空工作区

检测 → 发现缺失 → 创建目录 → 写入 WORKSPACE.md → 自我引导（铁律 #1/#2/#3）→ 持久化到 AGENTS.md → 记录日志

**产出：**
```
根目录/
├── projects/
├── tasks/
├── outputs/
│   ├── images/
│   └── docs/
├── knowledge/
├── scripts/
├── memory/
│   └── YYYY-MM-DD.md    ← 初始化日志
├── tmp/
├── WORKSPACE.md          ← 持久化规范文件
├── SOUL.md
├── USER.md
└── ...其他系统配置
```

### 重启后新智能体进入同一工作区

新智能体 → 检测 → 发现 WORKSPACE.md 和所有目录已存在 → 跳过 Step 2-3 → 完成。

**重启时依赖 WORKSPACE.md（目录规范）和 AGENTS.md（配置修改规范）作为持久化规范来源**，而不是依赖 SKILL.md（技能可能不在新智能体的技能列表中）。
