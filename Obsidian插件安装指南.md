# Obsidian 社区插件安装指南

> ⚠️ 使用社区插件前，需要先关闭 Obsidian 的「安全模式」

---

## 🔧 第一步：关闭安全模式

1. 打开 Obsidian → 左下角 ⚙️ **设置**
2. 左侧 → **社区插件**
3. 点击 **安全模式** 右侧开关 → 关闭
4. 点击 **浏览** 搜索插件

---

## 📦 推荐插件列表（按优先级排序）

### 🔥 必装（复盘核心）

| 插件 | 功能 | 搜索关键词 |
|------|------|-----------|
| **Calendar** | 日历视图，双击日期直接创建并打开每日笔记 | `calendar` |
| **Periodic Notes** | 支持创建每日/每周/每月笔记，可设不同模板 | `periodic notes` |
| **Vault Statistics** | 仓库统计，画活动热力图，直观看到哪天写了没写 | `vault statistics` |

### ⭐ 强烈推荐

| 插件 | 功能 | 搜索关键词 |
|------|------|-----------|
| **Dataview** | 数据查询引擎，可汇总统计每日评分、任务完成率等 | `dataview` |
| **Tracker** | 基于 Dataview 做指标追踪，自动绘制评分趋势图 | `tracker` |
| **Heatmap Calendar** | 类似 GitHub 贡献图的热力图日历，看每日活跃度 | `heatmap calendar` |
| **Tasks** | 增强型任务管理，支持截止日期、重复任务、状态追踪 | `obsidian tasks` |
| **Minimal Theme Settings** | 简洁主题，配复盘场景清爽好看 | `minimal theme` |
| **Style Settings** | 更多自定义样式调整 | `style settings` |

### 👍 锦上添花

| 插件 | 功能 | 搜索关键词 |
|------|------|-----------|
| **Auto Note Mover** | 自动移动笔记到指定文件夹 | `auto note mover` |
| **Remember Cursor Position** | 打开笔记时记住上次滚动位置 | `remember cursor` |
| **Paste URL into Selection** | 选中文字后粘贴自动转为链接 | `paste url into` |
| **Highlightr** | 彩色高亮标记工具，方便标记重点 | `highlightr` |

---

## 🚀 安装步骤

### 方法一：在 Obsidian 内直接安装（推荐）

1. 设置 → **社区插件** → 点击 **浏览**
2. 搜索上面插件名称
3. 点击插件 → **安装**
4. 安装后点击 **启用**
5. 建议一次性装完，然后重启 Obsidian

### 方法二：如果网络慢，手动安装（BRAT 方式）

1. 设置 → **社区插件** → 安装 **BRAT** 插件
2. 复制插件的 GitHub 仓库地址粘贴进去
3. BRAT 会自动下载安装

---

## ⚙️ 安装后的关键配置

### Calendar + Periodic Notes 配置

装完 Calendar 和 Periodic Notes 后：

1. 设置 → **Periodic Notes** 
   - **Daily Note**：
     - 格式：`YYYY-MM-DD`
     - 文件夹：`Daily`
     - 模板：`Templates/每日复盘模板`
   - **Weekly Note**：
     - 格式：`YYYY-第WW周`
     - 文件夹：`Weekly`
     - 模板：`Templates/周复盘模板`
   - **Monthly Note**：
     - 格式：`YYYY年MM月`
     - 文件夹：`Monthly`
     - 模板：`Templates/月度复盘模板`

2. 设置 → **Calendar**
   - 勾选 "Set as default" 关联 Periodic Notes
   - 确认日记格式一致

### Dataview 配置

设置 → **Dataview** → **Enable JavaScript Queries**（开启以支持 Tracker 图表）

### Tasks 配置

设置 → **Tasks** → 建议勾选
- "Set Done Date"（完成时自动填入日期）
- "Global Task Filter"（全局任务过滤）

---

## 💡 使用这些插件后你能做的事

- 左侧日历直接点日期写日记
- 周/月复盘一键生成
- Dataview 查询：统计本月平均效率分、任务完成率
- Tracker 画折线图：心情/效率趋势一目了然
- 热力图：一眼看出哪段时间在坚持、哪段断档了
