# 版本变更历史

## v2.4 (2026-06-26)

### 修复
- **上周计划完成情况显示错误**：保存周报后不再被 auto-promote 覆盖，始终展示已保存的完成状态和原因
- **补充说明输入框无法保存**：`onchange` → `oninput`，打字即存，点击保存按钮不会丢失
- **Word导出源一致性**：`downloadWord()` 的"上周计划完成情况"改为从 `reportHistory` 读取，与页面显示一致

### 新增
- **周五保护**：`saveWeeklyReport()` / `downloadWord()` / `downloadPDF()` 增加当日星期检查，非周五操作弹出"非周报生成时间"提示

### 技术细节
- 保存流程：reportHistory 深拷贝 → auto-promote → 临时回填显示 → 恢复 auto-promoted 状态
- 版本号 2.3 → 2.4，migrateData 兼容 2.2/2.3 自动升级

---

## v2.3 (2026-06-10)

### 新增
- **周报按项目独立**：`weeklyReport` 从扁平结构改为 per-project 结构 `{projId: {...}}`
- `switchProject()` 切换项目时自动刷新周报 + 概况卡片
- `migrateData()` 兼容 v2.2 旧平面结构 → v2.3 per-project

### 修复
- 云端数据时序竞争：tryCloudLoad 回调中增加周报刷新逻辑
- 候选列表排除已完成流程的分类
- 新增"添加到所有"快捷选项（一键勾选所有未完成分类）

### 技术细节
- 验证：283/283 braces, 0 残留引用
- 种子版本同步升至 2.3

---

## v2.2 (2026-06-10)

### 新增
- **周报功能**（三目录）：
  - 一号目录：项目概况（可编辑，与看板页共享）
  - 二号目录：按日期去重推进记录 + 结算事项汇总表
  - 三号目录：下周推进计划（增删改）
- **上周计划完成情况**：状态选择 + 原因编辑
- **项目概况卡片**：在看板页 overview 下方独立展示
- Word(.doc) 下载 / PDF 打印
- 历史报告自动归档（保留最近 12 份）
- `migrateData()` 自动兼容 v2.1 → v2.2

### 修复
- 种子数据 `lastUpdated` 改为 `"2000-01-01T00:00:00.000Z"`（修复新设备无法加载云端数据）
- `DataManager.save()` 新增新设备防护（`!_hasLocalCache && !cloudLoaded` 时阻止保存）
- 新设备显示加载提示 + 5 秒超时降级
- 种子数据内容同步至当前云端版本

### 技术细节
- 新增 5 个函数：`initProjOverview`, `toggleProjOverviewEdit`, `saveProjOverview`, `cancelProjOverview`, `viewProjOverview`
- 花括号验证：273/273

---

## v2.1 (2026-06-09)

### 新增
- **项目级堵点**：`projectBlockingIssue` 字段
- **批量记录**：支持一次添加到多个分类
- **版本防降级**：`tryCloudLoad` 版本检查，云端版本 ≥ 本地版本才接受
- `migrateData()` 自动处理版本升级

### 修复
- 页面空白 Bug：删除重复变量声明（`nmCallback` let/var 冲突导致 SyntaxError）
- localStorage KEY 从 `kanban_data` 改为 `engineering_kanban_v3`（与旧 kanban.html 冲突）
- Gist 旧数据覆盖问题

### 技术细节
- 数据版本升至 2.1
- 新增 `version` 字段到 `appData`

---

## v2.0 (2026-06-08)

### 新增
- **多项目支持**：项目切换（临港排海管 / 前程路电力隧道）
- **分类看板**：环形进度可视化 + 堵点标签
- **审批工作流**：可自定义步骤，追踪当前阶段
- **推进记录**：增删改记录，反馈意见管理
- **编辑弹窗**：审批阶段切换、自定义工作流
- 项目堵点 / 分类堵点继承机制
- 版本号管理 → localStorage KEY 隔离

### 技术细节
- 数据版本 2.0
- 数据结构：`projects[] → categories[] → workflow[] + records[]`

---

## v1.0 (2026-06-08 之前)

### 初始版本
- 单项目管理
- 基本结算分类看板
- GitHub Pages 部署
- GitHub Gist 数据存储
- localStorage 降级

---

## 版本号规则

- 种子数据 `version` 必须 ≤ 云端数据 `version`
- 升级时先更新云端数据，再更新种子数据
- 每个版本的数据结构变更由 `migrateData()` 自动处理
