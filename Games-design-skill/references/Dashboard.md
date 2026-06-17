# Dashboard — 数据看板

> **设计来源**：[互动游戏_平台产品-UI-Kit](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit)  
> - 业务页节点 `2212:109109`（页面 · 数据看板）  
> - 筛选卡片 `2212:113982`（Component 51 · multi-row + 查询/重置）  
> - 数据概览区 `2212:110979`（交易数据 · 指标卡 + 折线图）  
> - 指标卡网格 `2212:110995`、折线图 `2212:111144`  
> **设计体系**：Semi Design（抖音开放平台 B 端）  
> **页面用途**：功能页下的**数据看板**：双层 Tabs + 外置筛选 + 指标卡概览 + 趋势折线图。生成页面时 **必读** [LayoutSpec.md](./LayoutSpec.md) §八 + [preview/LayoutSpec.html](../preview/LayoutSpec.html)（壳层）+ [assets/README.md](../assets/README.md) §3.4 + 本文件。  
> **HTML 预览**：[`../preview/Dashboard.html`](../preview/Dashboard.html)  
> **更新日期**：2026-06-05

---

## 0. 页面信息架构

| 层级 | 模块 | 说明 |
|------|------|------|
| L0 | 全局壳层 | 顶栏 Header（60px）+ 侧栏「导航max」（272px）+ 主内容区 `#F9F9F9`（见布局规范） |
| L1 | 页面标题 | **仅**「功能标题」24px，**无**面包屑、**无** intro 卡片 |
| L2 | 能力切换 | **Line Tabs**（能力一～四） |
| L3 | 功能切换 | **Button Tabs**（全部 / 功能二 / 功能三 …） |
| L4 | **筛选卡片** `filter-panel` | **独立白卡片**，多行筛选 + 查询/重置（**不在**数据区内） |
| L5 | **数据区** `data-panel` | 标题区（数据概览 + 注释 + 导出）→ 指标卡 4×2 → 折线图 |

### 核心模式：数据看板

| 规则 | 说明 |
|------|------|
| 物理分离 | 筛选 `2212:113982` 与数据 `2212:110979` 为 **两张** 白卡片，中间垂直间距 **16px** |
| 数据区内无筛选 | `data-panel` 内 **不含** 搜索/Select/查询/重置 |
| 指标可切换 | 点击指标卡切换**选中态**，折线图 legend 与选中指标联动 |
| 筛选形态 | 默认 **multi-row**（两行 + 查询/重置），见 §2.5 |

### 与相关模版差异

| 对比项 | 筛选指标在列表外 `1665:217264` | **本页 `2212:109109`** |
|--------|-------------------------------|----------------------|
| 页面标题区 | 仅标题，无 intro | **同左** |
| 面包屑 | 无 | **无** |
| Tabs | Line + Button | **Line + Button** |
| 筛选位置 | 独立 `filter-panel` | **独立 `filter-panel`** |
| 时间控件 | Radio 173×32 + DateTimeRange 344×32 | **仅** DateTimeRange 344×32（**无** Radio 快捷） |
| 主内容区 | 表格 + 分页 `info` | **指标卡网格 + 折线图** `data-panel` |
| 主区标题 | CardTittle「功能列表」 | **数据概览** + 竖线分隔注释 + 导出图标 |
| Button Tab 文案 | 全部 / 能力介绍 / 能力管理 | **全部 / 功能二 / 功能三** |

---

## 1. Design Tokens

### 1.1 色彩

| Token | 值 | 用途 |
|-------|-----|------|
| `palette/brand/--semi-brand-5` | `#1C5CFB` | 主按钮（查询）、激活 Tab、选中指标卡描边、图表强调 |
| `palette/brand/--semi-brand-0` | `#EDF3FF` | Button Tab 激活背景 |
| `color/button_primary-bg-default` | `#1c5cfb` | 查询按钮 |
| `usage/text/--semi-color-text-0` | `#1C1F23` | 标题、指标数值、legend 文案 |
| `usage/text/--semi-color-text-1` | `#1c1f23cc` / 80% | 指标卡标题（下划线虚线） |
| `usage/text/--semi-color-text-2` | `#1c1f2399` / 60% | 未激活 Tab、占位、环比标签「较昨日」 |
| `Light/usage/text/--color-text-0` | `#171A1C` | 数据概览主标题 |
| `文本、图标色/注释(Text-gray-3)` | `#969AA0` | 数据更新注释文案 |
| `UIKIT/Border/border-color-3` | `#BFC1C5` | 标题区竖向分隔线 |
| `color/select_input_placeholder-text` | `#1c1f2399` | Select / 日期占位 |
| `color/input_default-text-default` | `#1c1f23` | 日期已填 |
| `color/datepicker_range_input-bg-default` | `#2e32380d` | DateTimeRange 底 |
| `color/select-bg-default` | `rgba(46,50,56,0.05)` | Select 底 |
| `Light/color/grey/--grey-0` | `#F9F9F9` | 主内容区底、未选中指标卡底 |
| `LightGray/gray-3` | `#F5F5F5` | 导出按钮底 |
| `Light/color/white/--white` | `#FFFFFF` | filter / data 卡片 |
| `color/tabs_tab_line_default-border-default` | `#1c1f2314` | Line Tab 底线 |
| `Light/color/red/--red-5` | `#F93920` | 环比上升（+0.04%） |
| `图表基准色/图表-1` | `#3865E9` | 折线、面积渐变、legend 色条 |
| `Color` / 选中指标底 | `#1c5cfb14` → `rgba(28,92,251,0.08)` | 选中指标卡背景 |
| `蓝色` | `#1c5cfb` | 选中指标卡 1px 描边 |
| `Semi/palette/grey/--grey-1` | `#E6E8EA` | 图表网格线 |
| `L2` | `#EBEBEB` | 图表基线 |
| X/Y 轴刻度字 | `rgba(23,26,28,0.45)` | 图表坐标轴 |
| 重置按钮字 | `rgba(23,26,28,0.25)` | 次要按钮 |
| `Text/ConstTextInverse` | `#FFFFFF` | 查询按钮字 |
| `Light/usage/primary-light/--color-primary-light-default` | `#EAF5FF` | 侧栏子项选中背景 |

### 1.2 字体

**字体族**：`PingFang SC`（指标数值可用 `Douyin Sans` Bold 或系统等宽近似）。

| 样式 | 规格 | 用途 |
|------|------|------|
| `Header/header-3/CN-Semi Bold` | 24/600/32 | 页面「功能标题」 |
| `Header/header-6/CN-Semi Bold` | 16/600/22 | 「数据概览」主标题 |
| `Regular/font-size-2` | 14/400/22 | 数据更新注释 |
| `Paragraph/regular/CN-Semi Bold` | 14/600/20 | 激活 Tab、查询 |
| `Paragraph/regular/CN-Regular` | 14/400/20 | 正文、筛选标签 |
| `Paragraph/small/CN-Regular` | 12/400/16 | 指标标题、环比、坐标轴 |
| `Paragraph/regular/ZH-Bold` | 14/500/20 | 重置按钮 |
| 指标数值 | 24/Bold | Douyin Sans 或 600+ 近似，行高 normal |
| Legend | 12/400 | 折线图图例 |

### 1.3 间距

| 场景 | 值 |
|------|-----|
| **Header 底边 → 页面标题顶** | **32px**（内容区 `y=92`，Header `60px`） |
| 页面标题 → Line Tabs | **8px** |
| Line Tabs 栏 | 51px（`tabs-bar` + **`tabs-content` 10px**） |
| Line Tabs → Button Tabs | **16px** |
| Button Tabs 栏 | **36px**（bar）+ **10px**（content 占位） |
| Button Tabs → **filter-panel** | **16px** |
| **filter-panel → data-panel** | **16px** |
| filter / data 卡片 padding | **24px** |
| filter 内部 flex-wrap | 行 gap **16px** · 列 gap **24px** |
| 筛选两行间距 | **16px** |
| 时间标签 ↔ DateTimeRange | **12px** |
| 筛选标签 ↔ 控件 | **8px** |
| 查询 ↔ 重置 | **12px** |
| data-panel 内部 gap | **16px**（标题区 ↔ 数据区） |
| 标题区 → 指标网格 | **16px**（标题 32px + gap 至数据区 72−56=16） |
| 指标行 ↔ 指标行 | **16px** |
| 指标卡内 gap | **8px**（标题 ↔ 数值 ↔ 环比） |
| 指标网格 ↔ 折线图 | **16px** |
| 指标卡 padding | **16px** |
| 指标卡列 gap | **16px**（4 列等分 1026px） |
| 标题主文案 ↔ 竖线 ↔ 注释 | **12px** / **13px** |
| 图表 Y 轴 ↔ 绘图区 | **14px** |
| 图表 legend 色条 ↔ 文案 | **6px** |

### 1.4 圆角与高度

| Token | 值 |
|-------|-----|
| Input / Select / Button / Button Tab | 3px |
| filter-panel / data-panel | 12px |
| DatePicker range | 3px |
| **指标卡** | **8px** |
| 导出按钮 | 6px |
| 竖向分隔线 | 2px（宽 1px，高 12px） |
| Legend 色条 | 2px（10×3px） |
| 筛选控件行高 | 32px |
| 指标卡行高 | **107px** |
| 标题区行高 | 32px |
| 数据区总高 | **502px**（指标 230 + gap 16 + 图表 256） |
| 折线图绘图区 | **186px**（含 Y 轴） |
| 导出图标按钮 | 32×32px |

### 1.5 尺寸

| 元素 | 尺寸 |
|------|------|
| 画板（业务页） | 1440 × **1053px** |
| 内容框架 | 1072px 宽 |
| 页面标题 | 96×32 |
| **filter-panel** | 1072 × **128px**（内容 1024×80 + padding 24×2） |
| **data-panel** | 1074 × **598px**（内容区 1026 宽） |
| DateTimeRange | **344 × 32px** |
| 时间组（标签+Range） | **384px** 宽 |
| Select | **216 × 32px** |
| 查询 / 重置 | 各 **74 × 32px** |
| 筛选组（标签+Select） | **280px** 宽 |
| 单指标卡（4 列） | **≈244.5px** 宽（flex:1，gap 16） |
| 图表区 | 1026 × **256px** |
| Y 轴标签区 | **28px** 宽 |

---

## 2. 组件规范

### 2.1 Header / 导航max

与 [LayoutSpec.md](./LayoutSpec.md) 一致。侧栏示例态：**管理 → 用户反馈** 选中（`2212:109349`）。

### 2.2 页面标题

节点 `2212:109117`：

| 属性 | 值 |
|------|-----|
| 文案 | 功能标题 |
| 字体 | 24px Semibold，`#1C1F23`，行高 32px |
| 容器 | **无**白卡片包裹 |
| 与顶部导航栏间距 | 由 `layout-shell.css` `.content-area` 上内边距 **32px** 提供（业务 CSS 勿覆盖） |
| 与 Line Tabs 间距 | **8px** |

### 2.3 Tabs — Line（能力）`2212:109120`

| 属性 | 值 |
|------|-----|
| 项 | **能力一**（默认激活）、能力二、能力三、能力四 |
| 栏高 | 51px |
| 激活 | 底边 2px `#1C5CFB`，14px Semibold |
| 未激活 | 14px Regular 60% 色 |
| 项间距 | 24px |

### 2.4 Tabs — Button（功能）`2212:109121`

| 属性 | 值 |
|------|-----|
| 项 | **全部**（默认激活）、功能二、功能三 |
| 激活 | 背景 `#EDF3FF`，文字 `#1C5CFB` 14px Semibold，圆角 3px |
| 未激活 | 透明底，14px Regular 60% 色 |
| padding | 12px × 8px；项间距 **8px** |
| 与 Line Tabs 间距 | **16px** |

### 2.5 筛选区 — filter-panel `2212:113982`

#### 2.5.1 容器

| 属性 | 值 |
|------|-----|
| 节点 | `Component 51` / `filter-panel` |
| 背景 | `#FFFFFF`，圆角 **12px** |
| padding | **24px** |
| 布局 | `flex-wrap`，`gap: 16px 24px` |
| 内容区 | 1024px × 80px |

#### 2.5.2 与「筛选指标在列表外」差异

| 规则 | 本页 | 列表外页 |
|------|------|----------|
| 时间快捷 Radio | **无** | 有（昨日/近7天/近30天） |
| 时间组 | 标签「时间」+ DateTimeRange **344×32** | Radio + DateTimeRange |
| 标签 ↔ Range 间距 | **12px** | 8px（含 Radio 时更复杂） |

#### 2.5.3 默认布局

**第一行**

```
[时间 标签] —12px— [DateTimeRange 344] —24px— [筛选标题 + Select 216] —24px— [筛选标题 + Select 216]
```

**第二行**

```
[筛选标题 + Select 216] —24px— [筛选标题 + Select 216] —24px— [查询 74] [重置 74]
```

| 控件 | 说明 |
|------|------|
| DateTimeRange | `type="dateTimeRange"` · 开始日期 ~ 结束日期 · 后缀 `icon-calendar-clock.svg` **16×16**（Figma `2212:113984` · `semi-icons-calendar_clock`） |
| Select ×4 | 占位「请选择」 |
| 查询 | Primary `#1C5CFB` |
| 重置 | 底 `rgba(46,50,56,0.05)`，字 `rgba(23,26,28,0.25)` |

**Semi**：`DatePicker type="dateTimeRange"`、`Select`、`Button`

### 2.6 数据区 — data-panel `2212:110979`

#### 2.6.1 容器

| 属性 | 值 |
|------|-----|
| 节点名 | 交易数据 / `data-panel` |
| 背景 | `#FFFFFF`，圆角 **12px** |
| padding | **24px** |
| 内部布局 | `flex-direction: column`，`gap: 16px` |

#### 2.6.2 标题区 `2212:110980`

位于 `data-panel` `2212:110979` 内，padding 上 **24px** 后 `y=24`，宽 **1026px**，高 **32px**。

```
┌─ 标题区 32px（2212:110980）────────────────────────────── [下载 32×32] ─┐
│ 数据概览  │  注：每日10：00更新前一日数据              2212:110986          │
└──────────────────────────────────────────────────────────────────────────┘
```

| 元素 | 节点 | 规格 |
|------|------|------|
| 「数据概览」 | `2212:110982` | 16px Semibold `#171A1C`，行高 22px |
| 竖线 | `2212:110984` | 1×12px `#BFC1C5`（`UIKIT/Border/border-color-3`），圆角 2px |
| 注释 | `2212:110985` | 14px Regular `#969AA0`（`Text-gray-3`），行高 22px；与竖线间距 **13px** |
| 标题左组 | `2212:110981` | 宽 288px；主标题 ↔ 竖线 ↔ 注释 间距 **12px** / **13px** |
| 对齐 | — | `justify-content: space-between` |

#### 2.6.3 下载按钮（导出）— `2212:110986`

画板 `2212:110979` 标题区右侧下载控件（Figma 图层名「下拉选择器」，实为**数据导出**按钮）。

| 属性 | 值 |
|------|-----|
| 容器节点 | `2212:110986`（`2212:110980` 内，`x=994` `y=0`） |
| 图标glyph | `2212:110987`（容器内 `8px` 内边距，**16×16** 下载箭头） |
| 外尺寸 | **32×32px** |
| 背景 | `#F5F5F5`（`LightGray/gray-3`） |
| 圆角 | **6px** |
| 描边图标色 | `#333333`，`stroke-width: 1.33333`，圆角端点 |
| 图形语义 | 上箭头 + 竖线 + 底横线（下载/导出） |
| 资产文件 | [`icon-export.svg`](../assets/icons/icon-export.svg)（**32×32 完整画板**，含灰底与图标） |
| Semi React | `IconExport` / `IconDownload`（仅 16×16 图形时）；HTML 推荐整画板 `<img>` |

```html
<!-- 相对 preview 路径 -->
<button type="button" class="btn-export" aria-label="导出数据">
  <img src="../assets/icons/icon-export.svg" width="32" height="32" alt="" aria-hidden="true">
</button>
```

```tsx
import { IconExport } from '@douyinfe/semi-icons';

<Button
  theme="borderless"
  icon={<IconExport />}
  style={{ width: 32, height: 32, background: '#F5F5F5', borderRadius: 6 }}
  aria-label="导出数据"
/>
```

**生成约束**：使用 `icon-export.svg` 整画板资源，**勿**再单独叠 `32×32` 灰底（已含在 svg 内）；按钮容器背景保持 `transparent`，hover 可用 `opacity: 0.85`。

### 2.7 指标卡 — MetricCard `2212:110998`

#### 2.7.1 网格布局

| 属性 | 值 |
|------|-----|
| 列数 | **4** |
| 行数 | **2**（画板示例 8 张） |
| 列间距 | **16px** |
| 行间距 | **16px** |
| 单卡高度 | **107px** |
| 布局 | `display: grid; grid-template-columns: repeat(N, 1fr); gap: 16px`（N 由配置决定，默认 **4**） |
| 卡片数量 | **可配置**，`items.length` 任意；不足一行左对齐排列，自动换行 |

#### 2.7.2 卡片结构

```
┌─ MetricCard 107px ──────────────┐
│ 指标标题（12px，虚线下划线） [?] │
│ 24px 数值（Douyin Sans Bold）    │
│ 较昨日  +0.04%（红 #F93920）     │
└─────────────────────────────────┘
```

| 区块 | 规格 |
|------|------|
| 标题 | 12px Regular，`--semi-color-text-1`（80%），`text-decoration: underline dotted` |
| 帮助图标 | `icon-help-circle.svg` **14×14**（Figma `2212:111076` · `Character/help`），可选，与标题 gap **4px** |
| 数值 | **24px Bold** `#1C1F23` |
| 环比行 | 12px；标签 60% 色；变化值 `#F93920`（上升）/ 业务可扩展下降色 |
| 环比 gap | 标签 ↔ 数值 **8px** |
| 内边距 | **16px** |
| **文案对齐** | 标题、数值、环比 **全部左对齐**；`text-align: left` · `align-items: flex-start` |

#### 2.7.3 状态

| 状态 | 背景 | 边框 |
|------|------|------|
| **选中**（默认首张） | `rgba(28,92,251,0.08)` | 1px solid `#1C5CFB` |
| **未选中** | `#F9F9F9` | 无 / 透明 |
| hover（实现建议） | 略深灰底或保持未选中 | — |

**交互**：点击卡片 → 切换选中态 → 折线图 series / legend 联动更新。

#### 2.7.4 画板示例指标

| 行 | 指标 | 数值 | 环比 |
|----|------|------|------|
| 1 | 用户活跃数 ★ | 12 | +0.04% |
| 1 | 新增用户数 | 4 | +0.04% |
| 1 | 累计用户数 | 15,020 | +0.04% |
| 1 | 分享次数 | 55 | +0.04% |
| 2 | 打开次数（含 help） | 13 | +0.04% |
| 2 | 人均打开次数 | 7,020 | +0.04% |
| 2 | 人均停留时长 | 00:00:17 | +0.04% |
| 2 | 次均停留时长 | 00:00:29 | +0.04% |

#### 2.7.5 指标卡配置（`METRIC_CONFIG`）

**适用**：[Dashboard.html](../preview/Dashboard.html)（`applyMetricConfig`）

| 字段 | 类型 | 说明 |
|------|------|------|
| `columns` | number | 每行列数，**1～4**，默认 `4` |
| `defaultSelected` | number | 默认选中项下标，联动折线图 legend |
| `compareLabel` | string | 环比前缀文案，默认「较昨日」 |
| `items` | array | 指标列表，**数量任意**；`[]` 时隐藏网格 |

**单项 `items[i]`**

| 字段 | 必填 | 说明 |
|------|------|------|
| `label` | 是 | 指标标题 |
| `value` | 是 | 主数值 |
| `compare` | 否 | 环比变化值，如 `+0.04%`；省略则不展示环比行 |
| `compareLabel` | 否 | 覆盖全局环比前缀 |
| `help` | 否 | `true` 时展示 14×14 帮助图标 |

```javascript
// 示例：仅展示 4 个指标，3 列布局
METRIC_CONFIG.columns = 3;
METRIC_CONFIG.items = [
  { label: "用户活跃数", value: "12", compare: "+0.04%" },
  { label: "新增用户数", value: "4", compare: "+0.04%" },
  { label: "累计用户数", value: "15,020", compare: "+0.04%" },
  { label: "分享次数", value: "55" }
];
applyMetricConfig(METRIC_CONFIG);
```

### 2.8 折线图 — LineChart `2212:111144`

#### 2.8.1 容器

| 属性 | 值 |
|------|-----|
| 总高 | **256px**（图表 223 + legend 17 + gap） |
| 绘图区 | 1026 × 186px（含 Y 轴 28px） |
| 圆角 | 外层容器 8px（可选） |

#### 2.8.2 坐标轴

| 轴 | 规格 |
|----|------|
| Y 轴 | 刻度 1200 / 900 / 600 / 300 / 0；12px `rgba(23,26,28,0.45)`；右对齐，宽 28px |
| X 轴 | 02:00 … 24:00（2 小时间隔）；12px 45% 灰，居中 |
| 网格线 | 水平 5 条，`#E6E8EA` / `#EBEBEB`，2px 高视觉线 |
| 基线刻度 | X 轴下方短竖线标记 |

#### 2.8.3 折线与面积

| 属性 | 值 |
|------|-----|
| 线色 | `#3865E9`（`图表基准色/图表-1`） |
| 线型 | 平滑曲线（smooth line） |
| 面积 | 线下方浅色渐变填充（品牌蓝 8%～透明） |
| 线宽 | 约 2px |
| Tooltip | 设计稿含隐藏态 `2212:111192`，hover 展示多行指标（实现可选） |

#### 2.8.4 图例 Legend `2212:111208`

| 属性 | 值 |
|------|-----|
| 位置 | 图表下方水平居中 |
| 色条 | 10×3px `#3865E9`，圆角 2px |
| 文案 | 与当前**选中指标**一致（默认「用户活跃数」） |
| 字号 | 12px Regular `#1C1F23` |
| 色条 ↔ 文案 | **6px** |

**Semi 映射**：`Chart`（line + area）或 `@douyinfe/semi-charts` / ECharts 封装，色值对齐 token。

### 2.9 业务图标资产

HTML 使用 [assets/icons/](../assets/icons/) 下 svg 的 `<img>`；React 用 `@douyinfe/semi-icons` 同名组件。详见 [assets/README.md](../assets/README.md) §3.4 · **§2.3（SVG 须 UTF-8；`Encoding error` 时修复注释编码）**。

| 位置 | 文件 | 尺寸 | Semi React | Figma |
|------|------|------|------------|-------|
| DateTimeRange 后缀 | `icon-calendar-clock.svg` | 16×16 | `IconCalendarClock` | `2212:113984` |
| 数据概览 · 下载/导出 | `icon-export.svg` | 32×32 | `IconExport` | `2212:110979` → `2212:110986` / `2212:110987` |
| 指标卡 · 帮助 | `icon-help-circle.svg` | 14×14 | `IconHelpCircle` | `2212:111076` |
| Select 后缀 | `icon-chevron-down.svg` | 16×16 | — | 同 InfoQuery |
| Select 搜索前缀 | `icon-search.svg` | 16×16 | `IconSearch` | 同 InfoQuery |

```html
<img src="../assets/icons/icon-calendar-clock.svg" class="cal-icon" width="16" height="16" alt="" aria-hidden="true">
<button type="button" class="btn-export" aria-label="导出数据">
  <img src="../assets/icons/icon-export.svg" width="32" height="32" alt="" aria-hidden="true">
</button>
<img src="../assets/icons/icon-help-circle.svg" class="metric-help" width="14" height="14" alt="" aria-hidden="true">
```

---

## 3. 布局规则

### 3.1 垂直节奏（画板实测）

```
功能标题          y=0
    ↓ 8px
Line Tabs         y=40, h=51
    ↓ 16px
Button Tabs       y=67, h=46
    ↓ 16px
filter-panel      y=119, h=128
    ↓ 16px
data-panel        y=263, h=598
```

### 3.2 水平约束

| 规则 | 值 |
|------|-----|
| 主内容最大宽 | **1072px**，水平居中 |
| 卡片内内容宽 | **1024px**（1072 − 24×2） |
| 指标区内容宽 | **1026px**（data-panel 内实测，实现可用 100%） |
| 侧栏固定宽 | 272px，内容区 `margin-left` 由 layout-shell 处理 |

### 3.3 响应式建议

| 断点行为 | 说明 |
|----------|------|
| 指标网格 | `< 900px` 可降为 2 列；`< 600px` 1 列 |
| 筛选区 | 保持 `flex-wrap`，查询/重置 **同组不拆行** |
| 图表 | 宽度 100%，高度保持 256px 或同比缩放 |

---

## 4. 默认文案清单

| 区域 | 文案 |
|------|------|
| 页面标题 | 功能标题 |
| Line Tab | 能力一 / 能力二 / 能力三 / 能力四 |
| Button Tab | 全部 / 功能二 / 功能三 |
| 筛选 | 时间 / 筛选标题 / 请选择 / 开始日期 ~ 结束日期 |
| 按钮 | 查询 / 重置 |
| 数据标题 | 数据概览 |
| 注释 | 注：每日10：00更新前一日数据 |
| 指标（见 §2.7.4） | 用户活跃数 … 次均停留时长 |
| 环比 | 较昨日 / +0.04% |
| 图例 | 用户活跃数（随选中卡变化） |
| X 轴 | 02:00 … 24:00 |

---

## 5. 模版配置（生成页面时填写）

```yaml
template: data-dashboard
layout:
  doc: LayoutSpec.md
  content_max_width: 1072

page:
  breadcrumb: false
  intro: false
  page_title: "功能标题"

tabs:
  line: [能力一, 能力二, 能力三, 能力四]
  button: [全部, 功能二, 功能三]
  default_button: 全部

filter:
  placement: outside
  mode: multi-row
  card: { padding: 24, radius: 12 }
  time_group:
    label: 时间
    radio: false          # 本页无快捷 Radio
    date_range: { type: dateTimeRange, width: 344 }
  fields:
    - { type: select, label: 筛选标题, width: 216, count: 4 }
  actions:
    query: { label: 查询, width: 74 }
    reset: { label: 重置, width: 74 }

dashboard:
  container: data-panel
  title: 数据概览
  note: "注：每日10：00更新前一日数据"
  export: true
  metrics:
    columns: 4          # 每行列数 1～4；items 数量任意，自动换行
    default_selected: 0
    compare_label: 较昨日
    items:
      - { label: 用户活跃数, value: "12", compare: "+0.04%" }
      - { label: 新增用户数, value: "4", compare: "+0.04%" }
      - { label: 累计用户数, value: "15,020", compare: "+0.04%" }
      - { label: 分享次数, value: "55", compare: "+0.04%" }
      - { label: 打开次数, value: "13", compare: "+0.04%", help: true }
      - { label: 人均打开次数, value: "7,020", compare: "+0.04%" }
      - { label: 人均停留时长, value: "00:00:17", compare: "+0.04%" }
      - { label: 次均停留时长, value: "00:00:29", compare: "+0.04%" }

chart:
  type: line
  color: "#3865E9"
  height: 256
  x_labels: [02:00, 04:00, 06:00, 08:00, 10:00, 12:00, 14:00, 16:00, 18:00, 20:00, 22:00, 24:00]
  y_ticks: [1200, 900, 600, 300, 0]
  legend_bind: selected_metric
```

---

## 6. Semi 组件映射

| 设计 | Semi |
|------|------|
| Line / Button Tabs | `Tabs type="line"` / `Tabs type="button"` |
| 外置筛选 | `DatePicker` `dateTimeRange` + `Select` + `Button` |
| 指标卡 | 自定义卡片 + `Typography`；可选 `Tooltip`（help） |
| 折线图 | `Chart` line / area 或业务图表库 |
| 导出 | `Button` `theme="borderless"` + `IconExport` / `icon-export.svg` |

文档：[DatePicker](https://semi.bytedance.net/zh-CN/components/datePicker) · [Chart](https://semi.bytedance.net/zh-CN/charts/line)

---

## 7. 实现与 Agent 调用

```text
生成「数据看板」页：
1. LayoutSpec.md §八 + preview/LayoutSpec.html → 复制壳层
2. layout-shell.css + app-avatar.png → 引入
3. assets/README.md §3.4 → icon-calendar-clock · icon-export · icon-help-circle
4. 本文件 → 标题 + 双层 Tabs + filter-panel（§2.5）+ data-panel（§2.6–§2.8）
5. Dashboard.html → 仅在 .main 内外置筛选 + 指标卡 + 折线图
```

- **勿**将筛选写入 `data-panel`
- **勿**混用列表模版表格区（对比 [InfoQuery.md](./InfoQuery.md)）
- 指标选中态与图表 legend **必须联动**

---

## 8. 参考链接

- Figma 业务页：[2212:109109](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2212-109109)
- Figma 筛选卡片：[2212:113982](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2212-113982)
- Figma 数据区画板 `2212:110979`：[交易数据](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2212-110979)（标题区含下载按钮）
- Figma 下载按钮 `2212:110986`：[32×32 画板](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2212-110986) · 内图标 `2212:110987`
- Figma 折线图：[2212:111144](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2212-111144)
- Figma 时间筛选组：[2212:113984](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2212-113984)
- Figma 指标卡（含 help）：[2212:111071](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2212-111071)
- [assets/README.md](../assets/README.md) §3.4 · [LayoutSpec.md](./LayoutSpec.md) · [InfoQuery.md](./InfoQuery.md)

---

*Figma MCP：2212:109109、2212:113982、2212:110979、2212:110986、2212:110987、2212:110995、2212:111144、2212:113984、2212:111071 及子节点*
