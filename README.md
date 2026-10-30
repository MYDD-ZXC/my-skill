# My AI Agent Skills

> 自己写的 AI Agent 技能包合集 — 每个都能独立装、独立用，不绑定任何特定 Agent。

| Skill | 解决什么问题 | 类型 |
|---|---|---|
| **[xhs-research](./skills/xhs-research/)** | 小红书选品与爆文调研：判断一个品类能不能做、怎么复刻 | 商业调研方法论 |
| **[context-guardian](./skills/context-guardian/)** | AI 反复犯同样的错 | 上下文管理 |
| **[requirement-pilot](./skills/requirement-pilot/)** | 项目做着做着跑偏、模型不懂你的场景 | 上下文管理 |
| **[project-journal](./skills/project-journal/)** | 跨会话后不知道之前做到哪了 | 上下文管理 |

后三个组合起来是 **Project Context Engine**（见[下文](#project-context-engine)）；第一个是独立的调研方法论。

---

# xhs-research

> 一套能真正落地的选品方法论：**先确认赛道有成交承接，再去找爆文**。

完整规则见 [SKILL.md](./skills/xhs-research/SKILL.md)，快速了解见 [详情页](./skills/xhs-research/README.md)。

## 为什么需要它

小红书上「这个品能不能做」，90% 的人顺序做错了：

看到一篇爆文 → 觉得有需求 → 上架 → 卖不动。

真正的原因是：**你看到的爆文，和这个类目的成交渠道可能根本不是一回事。**

实测数据：某家居类目全年 GMV ¥46,000 / 359 单，其中**床品套件的 88% 销售额来自直播**，笔记只贡献 4.4%。在这种类目里研究爆文怎么写，等于在没水的地方打井。

所以第一条铁律是：

> **先看成交载体（直播 / 笔记 / 商卡 谁在卖），再决定要不要看内容。**

## 五层漏斗

| 层 | 回答什么问题 | 数据源 | 频率 | 成本 |
|---|---|---|---|---|
| **L1 赛道** | 靠直播还是靠笔记成交？ | 乘风·类目洞察 → 成交载体 | 月更 | 0 |
| **L2 商品** | 哪些品在卖、价格带、头部集中度 | 乘风·商品洞察 100 条 | 周更 | 0 |
| **L3 爆文** | 哪些是爆款、怎么爆的 | 站内搜索（最多收藏 + 时间窗） | 日更 | 0 |
| **L4 内容** | 挂了什么品、观众买不买 | 笔记详情 + 评论区（需 xsec_token） | 日更 TOP10 | 0 |
| **L5 商品** | 价格 / 已售 / 上新 / 商品 ID | **云手机 App（Web 拿不到）** | 周更 | 设备费 |

每层都有硬门槛，不达标不进下一层：

| 层 | 门槛 | 不达标 |
|---|---|---|
| L1 | 直播销售额占比 ≤70% | 弃赛道，换词 |
| L2 | 头部 TOP10 销量占比 <60% | 换细分，别硬打 |
| L3 | 30 日内爆款 ≥3 篇 | 先确认是洼地还是没需求 |
| L4 | 意向密度 >25% | 换内容角度重测 |
| L5 | 可复刻性四问全过 | 放弃 |

## 两个反直觉的结论

**1. 收藏比点赞更能预测成交。**
L3 的第一排序用「最多收藏」，不是「最多点赞」。实测 **收藏→订单的转化率是 点赞→订单的 3 倍**。收藏意味着「我要回来再看一眼」，这是购买的前置动作；点赞只是「不错，划走」。

**2. 推荐流不能当品类采样器。**
云手机里滑商品详情页的「推荐」Tab，一屏 4 个商品，看起来很适合采集。但它是**算法全局推荐**，不是同品类。

实测：在牛奶绒四件套的商品页滑 12 屏采到 19 个样本，里面是手机壳、粉底液、香薰、餐具盲盒——家纺只占 3/19。算出的价格分位 P10=8.9 / 中位=21.8 / P90=129，完全不能代表四件套。

正确做法：搜索 → 切「商品」Tab → 按「销量」排序 → 再滑。

## 🔴 通道铁律：用云手机，不要用浏览器自动化

这条是踩过事故写下来的。

**2026-10-26**：用 Playwright 驱动浏览器抓小红书搜索页和笔记详情，短时间翻多篇笔记，**触发了平台风控——账号异常，浏览器被强制关闭**。

| 维度 | Web 自动化（Playwright/CDP） | 云手机（App + ADB） |
|---|---|---|
| 设备指纹 | 桌面 Chrome + CDP 端口，**可被识别** | 真实 Android 设备、真实 App |
| 行为特征 | 无鼠标轨迹、翻页节奏机器化 | 真实触摸事件，节奏可控 |
| 登录态 | 直接暴露主号 Cookie | 独立设备 + 独立 IP + 独立登录 |
| 请求频率 | 一次详情页 = 一串 XHR | 一次点击 = 一次正常页面加载 |
| 被封代价 | **封主号，店铺权重一起没** | 封小号，换设备即可 |

- 站内采集（搜索 / 笔记 / 详情 / 评论 / 商品）**只用云手机 ADB**
- **禁止** Playwright / CDP / Selenium / requests 直连小红书
- Web 端唯一允许的用法：**你自己打开页面，Agent 被动读取**（这时不算自动化）
- 出现验证码 / 滑块 / 302 到 `/404/sec_xxx`：**立刻停手，24 小时内不再登录**

## 安装

```bash
cp -r skills/xhs-research ~/.your-agent/skills/
```

Skill 靠 `SKILL.md` 的 frontmatter 触发，无依赖：

```yaml
---
name: xhs-research
description: 小红书爆文与爆品调研（五层漏斗 + 云手机 ADB 直采）...
---
```

## 实战输出

每轮调研产出三件东西，缺一不可：

1. `compete.csv` — 商品级原始数据（price / sold / title / shop / likes）
2. `compete.txt` — 汇总（价格分位 / 价格带分布 / 销量分档 / 店铺集中度 / TOP15）
3. **结论** — 必须回答：价格带有空档吗？头部集中度能不能切进去？爆款密度是红海还是洼地？

没有第 3 条，前两个只是数据搬运。

---

<a id="project-context-engine"></a>
# Project Context Engine

> 🧠 AI Agent 的项目级智能上下文管理系统 — 让模型记住你、懂你、不再犯同样的错

## 为什么需要它

| 痛点 | 传统方案 | Project Context Engine |
|------|---------|----------------------|
| 😤 反复犯同样的错 | 手动在 CLAUDE.md 里记录 | **自动检测 → 记录 → 归档 → 召回** |
| 😵 项目做着做着跑偏了 | 无方案 | **跨任务需求漂移检测 + 主动反问** |
| 🤦 模型不懂你的场景 | 手动写规则 | **自动积累用户画像 + 场景适配** |
| 😶 跨会话后失忆 | 每次从零开始 | **任务总结 + 项目旅程回溯** |

## 三个 Skill 分工

| Skill | 解决什么 | 核心机制 |
|-------|---------|---------|
| **context-guardian** | 重复犯错 | 情绪信号检测 → 错误记录 → 归档 → 召回 |
| **requirement-pilot** | 需求跑偏、不懂用户 | 意图收敛 → 漂移检测 → 场景画像 |
| **project-journal** | 历史不可追溯 | 任务总结 → 最近摘要 → 项目旅程 |

## 特性

- 🔄 **跨平台兼容** — 任何支持文件读写的 Agent 都能用（Claude Code、Cursor、Windsurf、WorkBuddy 等）
- 📄 **全 Markdown** — 所有数据人可读，可手改、可版本控制
- 🧠 **渐进式增强** — 有跨会话记忆的 Agent 能吃满，没有也不影响核心功能
- 📦 **轻量** — 三个 Skill 各自独立，可单装可组合

## 快速开始

```bash
# 1. 复制 Skill 目录
cp -r skills/context-guardian skills/requirement-pilot skills/project-journal ~/.your-agent/skills/

# 2. 初始化规范文件
cp templates/project-spec-template.md project-spec.md
```

填好 `project-spec.md` 的基本信息即可。三个 Skill 会自动在这些时机工作：

| 时机 | 触发的 Skill | 做什么 |
|------|-------------|--------|
| 任务开始前 | requirement-pilot | 记忆召回 → 漂移检查 → 意图收敛 |
| 任务开始前 | context-guardian | 加载已知错误记录 |
| 检测到情绪信号 | context-guardian | 记录错误 → 更新规范文件 |
| 发现需求偏移 | requirement-pilot | 向用户确认是否调整方向 |
| 任务结束时 | project-journal | 写任务总结 → 更新最近任务 |

## 规范文件结构

三个 Skill 共享同一个 `project-spec.md`，各自维护不同 section：

```
project-spec.md
├── 项目背景          ← requirement-pilot
├── 核心目标          ← requirement-pilot
├── 核心目标演变       ← project-journal
├── 用户画像          ← requirement-pilot
├── 场景画像（当前任务） ← requirement-pilot
├── 错误记录          ← context-guardian
│   ├── 活跃错误
│   └── 已归档
├── 最近任务          ← project-journal
└── 任务历史          ← project-journal
```

## 三级存储架构

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

## FAQ

**三个 Skill 必须一起用吗？**
不是。每个都能独立装：只装 `context-guardian` 就只解决重复犯错，只装 `project-journal` 就只解决任务历史。组合效果最佳。

**需要什么 Agent？**
任何能读写项目目录文件、能执行对话指令的 Agent 都行。不需要跨会话记忆、不需要特殊工具、不需要 API。

**规范文件会越来越大吗？**
它是「活跃记忆」，只保留当前有效内容：过期错误归档到 `archive/`，旧任务归档到 `archive/task-history/`，归档过多则 FIFO 淘汰。

**情绪信号检测会误判吗？**
会。所以记录时区分根因（模型输出问题 / 环境问题 / 待定），你也可以手动编辑规范文件删掉误判项。

**能用于非编程场景吗？**
可以。规范文件不绑编程：写作用它记风格偏好、调研用它记方向演变与已排除线索、设计用它记规范与反馈模式。

---

## 目录结构

```
my-skill/
├── README.md
├── LICENSE
├── CHANGELOG.md
├── skills/
│   ├── xhs-research/          # 小红书爆文爆品调研
│   │   ├── SKILL.md
│   │   └── README.md
│   ├── context-guardian/      # 错误记忆与防范
│   ├── requirement-pilot/     # 需求引导与场景适配
│   └── project-journal/       # 任务日志与项目回溯
└── templates/
    ├── project-spec-template.md
    └── archive-template.md
```

## License

MIT

## 贡献

欢迎 Issue 和 PR。

---

## English

**My AI Agent Skills** — a collection of self-written AI Agent skills, each installable and usable independently, bound to no specific agent.

| Skill | What it solves |
|---|---|
| **xhs-research** | Xiaohongshu (RED) product research: a 5-layer funnel that first checks *where sales actually come from* (livestream vs. notes) before studying viral content. Includes a hard-won rule: use a cloud phone with ADB for in-app data collection, never browser automation — it got the main account flagged once. |
| **context-guardian** | The model keeps making the same mistakes |
| **requirement-pilot** | Projects drift away from original goals; the model doesn't understand your context |
| **project-journal** | No record of what was done in previous sessions |

The last three form **Project Context Engine**: project-level context management using plain Markdown files — error memory, requirement drift detection, and task journaling. Works with any agent that can read and write files.
