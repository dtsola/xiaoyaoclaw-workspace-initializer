# OpenClaw Workspace Initializer 🏠

<p align="center">
  <img src="./assets/readme/hero.svg" width="100%" alt="OpenClaw Workspace Initializer — give every OpenClaw agent a home: standard directory structure, WORKSPACE.md rules, multi-agent config safety">
</p>

> 给每个 OpenClaw agent 一个「家」：标准目录结构 + WORKSPACE.md 规范 + 多 agent 配置安全。
> OpenClaw workspace initialization & standardization — the agent home your agents deserve.

![license](https://img.shields.io/badge/license-MIT-green)

## 为什么需要它

OpenClaw agent 每次会话都是全新启动。没有工作区规范，你的 agent 会：
- ❌ 把文件乱扔在根目录，越攒越乱
- ❌ 忘记自己的记忆系统（`memory/`），每次醒来都失忆
- ❌ 在多 agent 共享配置时，用 `config.apply` 整份覆盖 openclaw.json，**抹掉其他 agent 的修改**

这个 skill 一次性解决：**目录结构 + 持久化规范 + 配置安全**。

## 特性

- 🗂️ 标准目录结构：`projects/` `tasks/` `outputs/` `knowledge/` `scripts/` `memory/` `tmp/`
- 📜 WORKSPACE.md 持久化规范：重启后依然生效的目录管理规则
- 🛡️ 多 agent 配置安全：只用 `config.patch`，禁止 `config.apply` 覆盖他人修改
- ⚖️ **技能路径冲突仲裁**：其他技能若约定与 WORKSPACE.md 冲突的输出路径，一律以工作区规范为准，由执行 agent 完成路径翻译（如 `~/Downloads/research/<topic>` → `tasks/<topic>/`）——装再多技能也不乱

## 安装

```bash
# ClawHub（推荐）
clawhub install xiaoyaoclaw-workspace-initializer

# 或从 GitHub 手动安装
git clone https://github.com/dtsola/xiaoyaoclaw-workspace-initializer
# 把 SKILL.md 和 templates/ 放到你的 skills 目录
```

## 使用

1. 把 skill 放到 OpenClaw 的 skills 目录
2. 首次进入（或重置）一个工作区时，agent 会自动：
   - 创建 `projects/ tasks/ outputs/ knowledge/ scripts/ memory/ tmp/`
   - 写入 `WORKSPACE.md` 目录规范（重启后依然生效）
   - 检测多 agent 场景，把「config.patch ✅ / config.apply ❌」安全规范持久化进 `AGENTS.md`
   - 在 `memory/` 记录初始化日志

## 🚀 快速上手（三步，10 分钟）

以飞书 Bot 接入为例，三步让你的 agent 拥有一个规范的家：

### Step 1：接入你的 agent

新建一个智能体并接入（飞书 Bot / 微信 / Telegram 等均可），和它完成身份确认——起名字、确定称呼、划定职责范围：

![Step 1 - 全新 agent 接入](assets/quickstart-step1-new-agent.png)

### Step 2：一句话触发初始化

对你的 agent 说：

> 初始化你的工作目录，使用 xiaoyaoclaw-workspace-initializer

agent 会自动完成：检测缺失目录 → 创建 `projects/ tasks/ outputs/ knowledge/ scripts/ memory/ tmp/` → 写入 `WORKSPACE.md` 规范 → 把「Read WORKSPACE.md」启动规则与配置安全规范写入 `AGENTS.md` → 记录初始化日志：

![Step 2 - 执行初始化](assets/quickstart-step2-init-workspace.png)

### Step 3：验收成果

打开 agent 的工作区目录（Windows 资源管理器示例），标准结构已就位：

![Step 3 - 初始化完成](assets/quickstart-step3-workspace-ready.png)

> 💡 之后每次会话，agent 都会先读 `WORKSPACE.md` 再干活——目录不乱、记忆不丢、配置不打架。

## 与其他方案的区别

| | openclaw-workspace-starter | **xiaoyaoclaw-workspace-initializer** |
|---|---|---|
| 目录结构 | 基础模板 | 完整规范体系（7 目录 + 命名规范 + 行为规则） |
| 持久化规范 | 无 WORKSPACE.md 规范 | ✅ WORKSPACE.md 重启后持续生效 |
| 多 agent 配置安全 | ❌ | ✅ config.patch / 禁 config.apply（实战踩坑沉淀） |
| 模板独立 | 内嵌 | ✅ templates/ 可单独复用 |

**实战验证**：本规范来自 7 个 agent 共享单份 openclaw.json 的真实生产环境，`config.apply` 覆盖事故（openclaw.json.bak* 痕迹）是血泪教训。

## 姊妹项目

- 🧠 **xiaoyaoclaw-memory-distill**（记忆整理工具）：把对话蒸馏成结构化记忆——根目录 MEMORY.md + memory/ 每日日志，解决上下文溢出；MEMORY.md 缺失时自动从历史日志「首次建忆」。<https://github.com/dtsola/xiaoyaoclaw-memory-distill>

## 目录结构

```
xiaoyaoclaw-workspace-initializer/
├── SKILL.md                    # 技能主体（工作流 Step 1-5）
├── templates/
│   ├── WORKSPACE.md            # 目录规范模板
│   └── AGENTS-config-safety.md # 多 agent 配置安全规范
├── README.md
└── LICENSE
```

## License

MIT — 随便用，署名可选。

---

## 🛠️ 需要定制？

**Agent & Skills 定制，价格 ¥800 起。**

- 微信：`dtsola`（添加好友时备注：**openclaw定制**）
- 服务范围：OpenClaw 多 agent 部署 / 工作区规范化 / 自定义 Skill 开发 / agent 记忆系统搭建

## 小遥Claw

**小遥Claw，把 AI 助手装进自己的电脑。**

- 🚀 宣传页：<https://www.yuque.com/dtsola/igp1aa/adcicbai2zlem0bz>
- 📖 介绍页：<https://github.com/dtsola/xiaoyaoclaw-introduction>

## 关于作者

- 🌐 博客：<https://www.dtsola.com>
- 📺 B站：<https://space.bilibili.com/736015>
- 💻 GitHub：<https://github.com/dtsola>
- 📕 小红书：<https://www.xiaohongshu.com/user/profile/5b4c0597e8ac2b06aa13346d>

## 💬 加入交流群

小遥全系产品用户交流群——产品反馈 · 使用交流 · 功能建议：

<p align="center">
  <img src="./assets/readme/community-qr.png" width="280" alt="小遥AI 用户交流群二维码：扫码加群，或添加微信 dtsola（备注：加群）">
</p>

<p align="center">扫码加群，或添加微信 <code>dtsola</code>（备注：<b>加群</b>）</p>
