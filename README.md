# Project Context Engine

> 🧠 AI Agent 的项目级智能上下文管理系统 — 让模型记住你、懂你、不再犯同样的错

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

[English](#english) | 中文

---

## 为什么需要它？

你的 AI Agent 是否有这些问题？

| 痛点 | 传统方案 | Project Context Engine |
|------|---------|----------------------|
| 😤 反复犯同样的错 | 手动在 CLAUDE.md 里记录 | **自动检测 → 记录 → 归档 → 召回** |
| 😵 项目做着做着跑偏了 | 无方案 | **跨任务需求漂移检测 + 主动反问** |
| 🤦 模型不懂你的场景 | 手动写规则 | **自动积累用户画像 + 场景适配** |
| 😶 跨会话后失忆 | 每次从零开始 | **任务总结 + 项目旅程回溯** |

> 类似项目: [blader/napkin](https://github.com/blader/napkin) (错误记忆) | [rohitg00/pro-workflow](https://github.com/rohitg00/pro-workflow) (自纠正记忆) | [codejunkie99/agentic-stack](https://github.com/codejunkie99/agentic-stack) (跨 Agent 记忆)
>
> 本项目将**错误记忆 + 需求漂移检测 + 任务日志**三者融合为一个轻量 Skill 包，且不绑定任何特定 Agent。

---

## 它解决什么问题？

你是否遇到过这些场景：

- 😤 **"怎么又犯这个错了？"** — 模型反复犯同样的错误，你反复解释同样的问题
- 😵 **"我之前说的需求不是这个啊"** — 项目做着做着就跑偏了，做完才发现不是当初想要的
- 🤦 **"你根本不了解我的情况"** — 模型拿到需求就开干，不知道你是谁、给谁看、什么场景
- 😶 **"之前做到哪了？"** — 跨会话后完全不知道之前的进度，每次从零开始

**Project Context Engine** 通过三个 Skill 解决这些问题：

| Skill | 解决什么 | 核心机制 |
|-------|---------|---------|
| **context-guardian** | 重复犯错 | 情绪信号检测 → 错误记录 → 归档 → 召回 |
| **requirement-pilot** | 需求跑偏、不懂用户 | 意图收敛 → 漂移检测 → 场景画像 |
| **project-journal** | 历史不可追溯 | 任务总结 → 最近摘要 → 项目旅程 |

---

## 特性

- 🔄 **跨平台兼容** — 任何支持文件读写的 AI Agent 均可使用（Claude Code、Cursor、Windsurf、WorkBuddy 等）
- 📄 **全 Markdown** — 所有数据为人可读的 Markdown 文件，可手动编辑、可版本控制
- 🧠 **渐进式增强** — 有跨会话记忆的 Agent 可利用，没有的也不影响核心功能
- 📦 **轻量级** — 三个 Skill 各自独立，可单独安装，也可组合使用
- 🎯 **MVP 优先** — 先跑通核心价值，再迭代增强

---

## 快速开始

### 安装

将三个 Skill 目录复制到你的 Agent 的 Skills 目录中：

```
# 方式一：整体安装（推荐）
将 project-context-engine/ 整个目录放到你的 Agent 项目中

# 方式二：按需安装
只复制你需要的 Skill 目录（如只装 context-guardian）
```

### 初始化

在项目根目录下创建规范文件：

```bash
# 复制模板
cp templates/project-spec-template.md project-spec.md
```

然后根据项目实际情况填写 `project-spec.md` 中的基本信息。

### 使用

安装后，三个 Skill 会自动在以下时机工作：

| 时机 | 触发的 Skill | 做什么 |
|------|-------------|--------|
| 任务开始前 | requirement-pilot | 记忆召回 → 漂移检查 → 意图收敛 |
| 任务开始前 | context-guardian | 加载已知错误记录 |
| 执行中检测到情绪信号 | context-guardian | 记录错误 → 更新规范文件 |
| 执行中发现需求偏移 | requirement-pilot | 向用户确认是否调整方向 |
| 任务结束时 | project-journal | 写任务总结 → 更新最近任务 |

---

## 规范文件结构

三个 Skill 共享同一个 `project-spec.md` 文件，各自负责不同的 section：

```
project-spec.md
├── 项目背景          ← requirement-pilot 维护
├── 核心目标          ← requirement-pilot 维护
├── 核心目标演变       ← project-journal 维护
├── 用户画像          ← requirement-pilot 积累
├── 场景画像（当前任务） ← requirement-pilot 每次更新
├── 错误记录          ← context-guardian 维护
│   ├── 活跃错误
│   └── 已归档
├── 最近任务          ← project-journal 维护
└── 任务历史          ← project-journal 维护
```

---

## 版本规划

### v0.1（当前版本 — MVP）

- ✅ 情绪信号触发错误记录
- ✅ 错误过期归档 + 摘要留存 + 按需召回
- ✅ 分层意图收敛（每轮最多 3 个问题）
- ✅ 需求漂移检测（跨任务对比 + 反问确认）
- ✅ 任务总结 + 最近任务摘要

### v0.2（计划中）

- 用户画像自动积累
- 场景适配（输出风格自动调整）
- 记忆召回（跨会话搜索集成）
- 项目旅程视图
- FIFO 归档淘汰

### v0.3（远期）

- 加权淘汰机制（频率 + 活跃度 + 严重程度）
- 情绪词库可扩展 + 多语言支持
- 任务上下文自动分类
- 归档文件定期整理

---

## 与现有方案对比

| 维度 | CLAUDE.md | mem0 | Acontext | **Project Context Engine** |
|------|-----------|------|----------|---------------------------|
| 跨会话记忆 | 手动维护 | 自动 | 自动 | **自动 + 结构化** |
| 错误防范 | ❌ | ❌ | 部分 | **✅ 专门的错误模式库** |
| 需求漂移检测 | ❌ | ❌ | ❌ | **✅ 跨任务对比 + 反问** |
| 场景适配 | 手动规则 | ❌ | ❌ | **✅ 自动积累 + 适配** |
| 任务历史 | ❌ | ❌ | ❌ | **✅ 自动记录 + 回溯** |
| 跨平台 | Claude only | 框架级 | Claude only | **✅ 任意 Agent** |
| 人可读性 | ✅ | ❌ | ✅ | **✅ 全 Markdown** |

---

## 目录结构

```
project-context-engine/
├── README.md                          # 本文件
├── skills/
│   ├── context-guardian/              # Skill 1: 错误记忆与防范
│   │   └── SKILL.md
│   ├── requirement-pilot/             # Skill 2: 需求引导与场景适配
│   │   └── SKILL.md
│   └── project-journal/               # Skill 3: 任务日志与项目回溯
│       └── SKILL.md
└── templates/
    ├── project-spec-template.md       # 规范文件模板
    └── archive-template.md            # 归档文件模板
```

---

## 工作原理

### 整体流程

```
┌─────────────────────────────────────────────────────────┐
│                    任务开始                               │
│                                                         │
│  ┌──────────────┐    ┌──────────────┐                   │
│  │ project-     │    │ context-     │                   │
│  │ journal      │    │ guardian     │                   │
│  │              │    │              │                   │
│  │ 读取最近任务  │    │ 加载错误记录  │                   │
│  └──────┬───────┘    └──────┬───────┘                   │
│         │                   │                           │
│  ┌──────▼───────────────────▼───────┐                   │
│  │       requirement-pilot          │                   │
│  │                                  │                   │
│  │  1. 记忆召回（读规范文件）         │                   │
│  │  2. 需求漂移检查                  │                   │
│  │  3. 意图收敛（如需要）            │                   │
│  └──────────────┬───────────────────┘                   │
│                 │                                       │
│         ┌───────▼───────┐                               │
│         │   执行任务     │                               │
│         └───────┬───────┘                               │
│                 │                                       │
│    ┌────────────┼────────────┐                          │
│    │            │            │                          │
│    ▼            ▼            ▼                          │
│ 情绪信号?    需求偏移?    任务完成?                      │
│    │            │            │                          │
│    ▼            ▼            ▼                          │
│ context-    requirement-  project-                      │
│ guardian    pilot         journal                       │
│ 记录错误    确认方向      写总结                         │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 三级存储架构

```
┌─────────────────────────────────┐
│  project-spec.md (活跃记忆)      │
│  - 当前有效错误 + 摘要           │  ← 日常使用
│  - 当前需求 + 用户画像           │
│  - 最近任务（5 条）              │
└──────────────┬──────────────────┘
               │ 过期/溢出时迁移
               ▼
┌─────────────────────────────────┐
│  archive/ (冷存储)               │
│  - 错误归档（完整记录）           │  ← 按需召回
│  - 任务历史归档                  │
│  - FIFO 淘汰                    │
└─────────────────────────────────┘
```

---

## 常见问题

### Q: 这三个 Skill 必须一起用吗？

不是。每个 Skill 都可以独立使用：
- 只装 `context-guardian` → 只解决重复犯错问题
- 只装 `requirement-pilot` → 只解决需求澄清和场景适配
- 只装 `project-journal` → 只解决任务历史记录

组合使用效果最佳。

### Q: 需要什么 Agent 才能用？

任何支持以下能力的 Agent 均可使用：
- 读写项目目录下的文件
- 在对话中执行指令（Skill 本质是 prompt 指令）

不需要跨会话记忆、不需要特殊工具、不需要 API。

### Q: 规范文件会越来越大怎么办？

规范文件设计为"活跃记忆"，只保留当前有效的内容：
- 过期错误 → 归档到 `archive/` 目录
- 旧任务记录 → 归档到 `archive/task-history/`
- 归档文件过多 → FIFO 淘汰（MVP 版本）

### Q: 情绪信号检测会不会误判？

会。用户说"怎么又错了"可能是模型的错，也可能是环境问题。解决方案：
- 记录时区分"根因归类"（模型输出问题 / 环境问题 / 待定）
- 用户可以手动编辑规范文件，删除误判的记录

### Q: 能不能用于非编程场景？

完全可以。规范文件不绑定编程场景：
- 写作项目 → 记录写作风格偏好、常见问题
- 调研项目 → 记录调研方向演变、已排除的线索
- 设计项目 → 记录设计规范、用户反馈模式

---

## License

MIT

---

## 贡献

欢迎提交 Issue 和 PR！

---

<a id="english"></a>

## English

**Project Context Engine** is a set of AI Agent skills that provide project-level intelligent context management. It solves three core problems:

1. **Repeated Errors** — The model keeps making the same mistakes (`context-guardian`)
2. **Requirement Drift** — The project drifts away from its original goals without anyone noticing (`requirement-pilot`)
3. **Lost History** — No record of what was done in previous sessions (`project-journal`)

### Key Features

- **Cross-platform**: Works with any AI Agent that supports file read/write
- **All Markdown**: Human-readable, editable, version-controllable
- **Lightweight**: Each skill is independent, install only what you need
- **Progressive Enhancement**: Uses Agent's cross-session memory if available, works without it

### Quick Start

1. Copy the skills to your Agent's skills directory
2. Copy `templates/project-spec-template.md` to your project root as `project-spec.md`
3. Start working — the skills will automatically manage context for you
