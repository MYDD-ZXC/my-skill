# Changelog

## v0.1.0 (2026-06-09) — MVP Release

### 🎉 Initial Release

三个核心 Skill 的 MVP 版本：

#### context-guardian — 错误记忆与防范
- ✅ 情绪信号检测（中文/英文关键词 + 语义匹配）
- ✅ 错误记录（归因判断：模型问题 / 环境问题 / 待定）
- ✅ 过期归档（30 天未复发 → 迁移到 archive/errors/）
- ✅ 按需召回（已归档错误复发时自动调取）
- ✅ FIFO 淘汰（归档文件超过 20 个时删除最早的）

#### requirement-pilot — 需求引导与场景适配
- ✅ 规范文件自动初始化（从模板创建 project-spec.md）
- ✅ 记忆召回（读取规范文件 + 可选的跨会话记忆搜索）
- ✅ 需求漂移检测（跨任务对比 + 偏离确认）
- ✅ 分层意图收敛（每轮最多 3 个问题，选择题优先）
- ✅ 场景画像 + 输出适配（术语/案例/格式/详细程度）
- ✅ 用户画像缓慢积累

#### project-journal — 任务日志与项目回溯
- ✅ 任务总结（任务结束时自动记录）
- ✅ 最近任务摘要（保留最近 5 条）
- ✅ 任务历史管理（保留最近 10 条，更早的归档到 archive/task-history/）
- ✅ 核心目标演变追踪

### 📦 附带文件
- `templates/project-spec-template.md` — 规范文件模板
- `templates/archive-template.md` — 归档文件模板
- `README.md` — 完整的使用文档（中英双语）
