# List — 信息查询（能力列表）

> **设计来源**：[互动游戏_平台产品-UI-Kit](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit)  
> - **单个能力列表** `1653:120277`  
> - **合集能力列表** `1634:68043`  
> - **筛选指标在列表外** `1665:217264`  
> - 表格/筛选规范板 `2185:84044` · 标签规范 `2186:90908`  
> **设计体系**：Semi Design（抖音开放平台 B 端）  
> **页面用途**：平台能力下的「列表管理 / 信息查询」落地页：能力概览、Tab、筛选、表格、分页。生成页面时 **必读** [LayoutSpec.md](./LayoutSpec.md) §八 + [preview/LayoutSpec.html](../preview/LayoutSpec.html)（壳层）+ [assets/README.md](../assets/README.md) §3.2 + 本文件。  
> **HTML 预览**：[`preview/InfoQuery.html`](../preview/InfoQuery.html)（`applyListPageConfig` · `LIST_PAGE_CONFIG`）  
> **更新日期**：2026-06-11

---

## 0. 模版选型

生成列表页前，按以下顺序判断变体（**决策优先级从高到低**）：

| 优先级 | 判定条件 | 选用变体 | Figma 节点 | `LIST_PAGE_CONFIG` |
|--------|----------|----------|------------|-------------------|
| 1 | 筛选区 **独立于列表卡片外**（`filter-panel`），或页面 **仅功能标题、无 intro** | **筛选指标在列表外** | `1665:217264` | `filterPlacement: "outside"` 或 `variant: "filter-outside"` |
| 2 | Tab **≥ 2 层**（子能力 Line + 功能 Button 等双层） | **合集能力列表** | `1634:68043` | `tabLevels ≥ 2` 或 `variant: "collection"` |
| 3 | 默认：单层 Line Tabs + intro | **单个能力列表** | `1653:120277` | `tabLevels: 1` · `filterPlacement: "inside"` |

**选型规则**：

1. **筛选在列表外** → 独立 `filter-panel` 白卡片 + `info` 内 **CardTittle**，列表内 **无** 工具栏  
2. **超过单层 Tab（≥ 2 层）** 且筛选在列表内 → **合集能力列表**（Line 子能力 + Button 功能）  
3. **仅 1 层 Line Tab** 且筛选嵌入列表 → **单个能力列表**  
4. **超过 2 级 Tab**（3 层及以上）：画板暂无独立模版，**默认沿用合集双层结构**（最外层 Line、内层 Button 类推）

```js
function resolveListVariant(cfg) {
  if (cfg.variant) return cfg.variant;
  if (cfg.filterPlacement === "outside") return "filter-outside";
  if ((cfg.tabLevels || 1) >= 2) return "collection";
  return "single";
}
```

### 0.1 变体差异速查

| 对比项 | 单个能力 `1653:120277` | 合集能力 `1634:68043` | 筛选外置 `1665:217264` |
|--------|------------------------|------------------------|------------------------|
| 面包屑 | 有 | 有 | **无** |
| 页面标题区 | intro 152px + **元信息** | intro 136px，**无**元信息 | **仅**「功能标题」，无 intro |
| Tabs | **单层** Line | Line + Button | Line + Button |
| 筛选位置 | 嵌入 `list-panel` | 嵌入 `info`（simple） | **独立** `filter-panel` |
| 筛选模式 | simple（默认）/ standard / multi-row | simple | **multi-row** + 查询/重置 |
| 时间控件 | 可选（standard/multi-row） | 无 | **Radio 173×32 + DateTimeRange 344×32** |
| 列表区标题 | 无 CardTittle | 无 | **CardTittle「功能列表」** |
| 列表容器名 | `list-panel` | `info` | `info` |
| 操作列宽 | 264px | 264px | **248px** |
| 主 CTA | 「新建能力」 | 「新建能力」 | 查询 / 重置（筛选卡内） |

### 0.2 页面信息架构（共用）

| 层级 | 模块 | 说明 |
|------|------|------|
| L0 | 全局壳层 | 顶栏 Header（60px）+ 侧栏「导航max」（272px）+ 主内容区 `#F9F9F9`（见布局规范） |
| L1 | 面包屑 / 页面标题 | 见各变体 §0.3–§0.5 |
| L2 | 能力概览 `intro` | 可选；见各变体 |
| L3+ | Tab 切换 | Line / Line+Button |
| Ln | 筛选区 | 列表内工具栏 或 独立 `filter-panel` |
| Ln+1 | 列表区 | `list-panel` 或 `info`：表格 + 分页 footer |

### 规范板 `2185:84044` 覆盖范围（三变体共用）

| 区块 | 节点 | 内容 |
|------|------|------|
| 单元格间距 | `2185:84293` | 默认行高 64、padding、多行自适应 |
| 表格组件库 | `2185:84095` | 表头、横/纵向操作行、分页、完整表格 |
| 筛选规范 | `2185:84143` | 交互注解、间距、多行筛选项 |
| 操作语义 | `2185:84287` | 常规 / 禁用 / 删除色 |
| 标签规则 | `2186:90908` | Tag（详情）/ StatesTag（列表）五态 + 尺寸 + 图标 |

---

### 0.3 单个能力列表 · 单层 Tab（`1653:120277`）

| 层级 | 模块 | 说明 |
|------|------|------|
| L1 | 面包屑 | 一级页面 / 二级页面 |
| L2 | 能力概览 `intro` | 能力标题 + 主描述 + **元信息**（适用 App、能力接入指引） |
| L3 | 功能切换 | **单层** Line Tabs（能力介绍 / 功能二～四） |
| L4 | 列表区 `list-panel` | 筛选工具栏 → 表格 → 分页 footer |

| 对比项 | 功能介绍 `1878:102218` | 本变体 |
|--------|------------------------|--------|
| Tabs 下内容 | 能力价值、文字描述、应用示例 | **筛选 + 表格 + 分页** |
| 主 CTA | 「开始接入」 | **「新建能力」**（工具栏右侧） |
| 默认激活 Tab | 能力介绍 | **能力介绍**（其下即列表） |

**intro**：白底、圆角 12px、padding 24、gap 16；24px 标题；14px 60% 描述；元信息行。

**筛选默认 simple**（`1653:120318`）：单行 32px；左：搜索 216 + 筛选标题 + Select 216；右：「新建能力」80×32。

---

### 0.4 合集能力列表 · 双层 Tab（`1634:68043`）

| 层级 | 模块 | 说明 |
|------|------|------|
| L1 | 面包屑 | 一级页面 / 二级页面 |
| L2 | 能力概览 `intro` | 能力标题 + 主描述（**无**适用 App / 能力接入指引） |
| L3 | 子能力切换 | **Line Tabs**（子能力一～四） |
| L4 | 功能切换 | **Button Tabs**（能力介绍 / **能力管理** / 功能三～四） |
| L5 | 列表区 `info` | 筛选工具栏 → 表格 → 分页 footer |

| 属性 | 值 |
|------|-----|
| intro padding | **32px 24px**（合集专用） |
| intro 高度 | **136px** |
| Line → Button Tabs | **16px** |
| Button Tabs 栏高 | **36px** |
| Button Tab 激活 | 背景 `#EDF3FF`，文字 `#1C5CFB` |
| 默认激活功能 Tab | **能力管理**（列表挂载此 Tab） |

**筛选**：嵌入 `info`，默认 **simple**（搜索 + 1 Select + 新建能力），无查询/重置。

---

### 0.5 筛选指标在列表外（`1665:217264`）

| 层级 | 模块 | 说明 |
|------|------|------|
| L1 | 页面标题 | **仅**「功能标题」24px，**无**面包屑、**无** intro |
| L2 | 能力切换 | **Line Tabs**（能力一～四） |
| L3 | 功能切换 | **Button Tabs**（**全部** / 能力介绍 / 能力管理 …） |
| L4 | **筛选卡片** `filter-panel` | **独立白卡片**，multi-row + 查询/重置 |
| L5 | 列表区 `info` | CardTittle「功能列表」→ 表格 → 分页 |

**核心模式**

| 规则 | 说明 |
|------|------|
| 物理分离 | 筛选与列表为 **两张** 白卡片，间距 **16px** |
| 列表内无筛选 | `info` 内 **不含** 搜索/Select/查询/重置/「新建能力」 |
| 列表标题 | **CardTittle** 16px Semibold「功能列表」 |
| 时间筛选 | RadioButtonGroup 173×32（昨日/近7天/近30天）+ DateTimeRange 344×32 |

**filter-panel 默认布局**

```
第一行：[时间组] —24px— [筛选标题 + Select 216] —24px— [筛选标题 + Select 216]
第二行：[筛选标题 + Select 216] —24px— [筛选标题 + Select 216] —24px— [查询 74] [重置 74]
```

**时间组** = 标签「时间」—8px— RadioGroup 173 —8px— DateTimeRange 344

---

## 1. Design Tokens

### 1.1 色彩

| Token | 值 | 用途 |
|-------|-----|------|
| `palette/brand/--semi-brand-5` | `#1C5CFB` | 主按钮、激活 Tab、查询、常规操作 |
| `palette/brand/--semi-brand-0` | `#EDF3FF` | Button Tab 激活背景、分页选中 |
| `color/button_primary-bg-default` | `#1c5cfb` | Primary 按钮 |
| `usage/danger/--semi-color-danger` | `#F93920` | 删除操作 |
| `palette/grey/--semi-grey-4` | `#888D92` | 禁用操作 |
| `usage/text/--semi-color-text-0` | `#1C1F23` | 标题、表体、筛选标签 |
| `usage/text/--semi-color-text-2` | `rgba(28,31,35,0.6)` | 描述、表头 TH、占位、footer 统计 |
| `Light/usage/text/--color-text-0` | `#171A1C` | StatesTag、Select 已选 |
| `color/select-bg-default` | `rgba(46,50,56,0.05)` | 控件、表头灰底 |
| `Light/color/grey/--grey-0` | `#F9F9F9` | 主内容区底 |
| `Light/color/white/--white` | `#FFFFFF` | 卡片、操作列底 |
| `color/tabs_tab_line_default-border-default` | `rgba(28,31,35,0.08)` | Tab 底线、表格行分割 |
| Tag / StatesTag 五态色 | 见 §2.8 | 三变体共用 |
| 重置按钮字 | `rgba(23,26,28,0.25)` | 次要按钮 |
| Radio 未选中字 | `rgba(23,26,28,0.65)` | 近7天 / 近30天 |

### 1.2 字体

**字体族**：`PingFang SC`（分页数字可用 Inter）。

| 样式 | 规格 | 用途 |
|------|------|------|
| `Header/header-3/CN-Semi Bold` | 24/600/32 | intro / 页面「功能标题」 |
| `Header/header-6/CN-Semi Bold` | 16/600/22 | CardTittle「功能列表」 |
| `Paragraph/regular/CN-Semi Bold` | 14/600/20 | 激活 Tab、主按钮、表头 TH、查询 |
| `Paragraph/regular/CN-Regular` | 14/400/20 | 正文、表体、面包屑、统计 |
| `Paragraph/small/CN-Semi Bold` | 12/600/16 | Tag Small |
| `CN/14_P1_Medium` | 14/500/20 | 行内操作链接 |
| `Paragraph/regular/ZH-Bold` | 14/500/20 | 重置按钮 |

### 1.3 间距

| 场景 | 值 | 变体 |
|------|-----|------|
| Header 底 → 内容顶 | **32px** | 全部 |
| 面包屑 → intro | 16px | 单个 / 合集 |
| intro → Line Tabs | **8px** | 全部有 intro 时 |
| 页面标题 → Line Tabs | **8px** | 筛选外置 |
| Line Tabs 栏 | 51px + **10px** tabs-content | 全部 |
| Line → Button Tabs | **16px** | 合集 / 筛选外置 |
| Button → 列表区顶 | **16px** | 合集 / 筛选外置 |
| filter-panel → info | **16px** | 筛选外置 |
| intro / list / filter / info padding | **24px** | 卡片内 |
| intro padding（合集） | **32px 24px** | 合集 |
| 列表内部 gap | **16px** | 工具栏 ↔ 表格 ↔ footer |
| 筛选标签 ↔ 控件 | **8px** | 全部 |
| 筛选项组 ↔ 组 | **24px** | 全部 |
| 查询 ↔ 重置 | **12px** | standard / multi-row |
| 工具栏 → 表头 | **16px** | 全部 |
| 表体 padding | **16px 12px** | 全部 |
| 横向操作 gap | **24px** | 全部 |

### 1.4 圆角与高度

| Token | 值 |
|-------|-----|
| Input / Select / Button / Tag / 分页 | 3px |
| intro / list / filter / info | 12px |
| 筛选控件行高 | 32px |
| 表头行高 | 36px |
| **表格默认行高** | **64px**（内容多时可自适应，示例 74px） |
| footer / 分页项 | 32px |
| Tag Small | 20px |
| StatesTag 圆点 | 8×8px |
| CardTittle 行高 | 32px |

### 1.5 尺寸

| 元素 | 尺寸 |
|------|------|
| 内容框架 | **1072px** 宽 |
| 搜索 / Select（默认） | 216 × 32px |
| DateTimeRange（筛选外置） | 344 × 32px |
| RadioButtonGroup | 173 × 32px |
| 查询 / 重置 | 各 74 × 32px |
| 主按钮「新建能力」 | 80 × 32px |
| **六列宽（默认）** | 200 / 160 / 120 / 120 / 160 / **264** = 1024px |
| **六列宽（筛选外置）** | 200 / 160 / 120 / 120 / 160 / **248** = 1008px |
| **九列宽（扩展）** | 200 / 220 / 120 / 160 / 160 / 160 / 160 / 160 / 260 = 1600px |

---

## 2. 组件规范

### 2.1 Header / 导航max / Breadcrumb

与 [LayoutSpec.md](./LayoutSpec.md) 一致。面包屑高 **28px**，分隔符 `/`。筛选外置变体 **无面包屑**。

### 2.2 intro（能力概览）

| 变体 | 节点 | 元信息 | padding |
|------|------|--------|---------|
| 单个能力 | `1657:157865` | **有**（`icon-mobile-app` + `icon-brief-stroked`，见 [assets/README.md](../assets/README.md) §3.2） | 24px |
| 合集能力 | `1651:114437` | **无** | **32px 24px** |

### 2.3 Tabs — Line

| 变体 | 默认项 |
|------|--------|
| 单个能力 | 能力介绍（默认）、功能二～四 |
| 合集能力 | 子能力一（默认）、子能力二～四 |
| 筛选外置 | 能力一（默认）、能力二～四 |

栏高 51px；激活底边 2px `#1C5CFB`；项间距 24px。

### 2.4 Tabs — Button（合集 / 筛选外置）

| 变体 | 默认项 |
|------|--------|
| 合集能力 | 能力介绍、**能力管理**（默认）、功能三～四 |
| 筛选外置 | **全部**（默认）、能力介绍、能力管理 |

激活：背景 `#EDF3FF`，文字 `#1C5CFB` 14px Semibold，圆角 3px；padding 8px 12px；项间距 8px。

### 2.5 筛选区

#### 2.5.1 交互规则（必须遵守）

| 规则 | 说明 |
|------|------|
| 时间置前 | DatePicker / DateTimeRange 放筛选条最左 |
| 标签与控件分离 | 「筛选标题」为独立 14px 文案，不进 Select 内 |
| Select 宽度 | 默认 **216px**；最窄 **120px** |
| 查询 / 重置 | 筛选项 ≥3 建议使用；各 **74px**；**整组折行**，不可只折一个 |
| 重置样式 | 底 `rgba(46,50,56,0.05)`，字 25% 黑，Medium 14px |
| 外置筛选 | 变更 **不** 写入 `info`；查询作用于表格数据源 |

#### 2.5.2 模式 A — simple（单个 / 合集默认）

嵌入 `list-panel` 或 `info`：单行 32px；左：搜索 216 + 筛选标题 + Select 216；右：「新建能力」。

#### 2.5.3 模式 B — standard（单行 + 查询重置 `2185:84149`）

```
[时间 + DateRange 296] —24px— [筛选标题 + Select 120] —24px— … — [查询 74] [重置 74]
```

#### 2.5.4 模式 C — multi-row（筛选外置默认 `1665:217963`）

两行筛选 + 查询/重置；`flex-wrap`，`gap: 16px 24px`；独立白卡片 padding 24px。

#### 2.5.5 时间快捷筛选 — RadioButtonGroup `1665:221255`

| 属性 | 值 |
|------|-----|
| 尺寸 | 173 × 32px |
| 轨道底 | `rgba(46,50,56,0.05)`，圆角 3px，padding 4px |
| 选中项 | 白底 + `#1C5CFB` 字 14px Semibold |
| 未选中 | 透明底 + `rgba(23,26,28,0.65)` |
| 与 DateRange 间距 | **8px** |

**Semi**：`RadioGroup type="button"` + `DatePicker type="dateTimeRange"`

### 2.6 CardTittle（仅筛选外置 `1665:218843`）

文案「功能列表」；16px / 600 / 行高 22px；`#1C1F23`；高 32px；位于 `info` 顶部。

### 2.7 表格

#### 2.7.1 表头

高 **36px**；背景 `rgba(46,50,56,0.05)`；TH 14px Semibold 60% 色；padding `8px 12px`。

**默认六列**

| 列 | 宽 | 表头 |
|----|-----|------|
| 1 | 200px | 标题 |
| 2 | 160px | 文本信息 |
| 3 | 120px | 任务状态 |
| 4 | 120px | 标签 |
| 5 | 160px | 创建日期 |
| 6 | 264px / **248px** | 操作 |

#### 2.7.2 单元格类型

| 类型 | 规格 |
|------|------|
| 普通文本 | 14px Regular `#1C1F23` |
| 图片+文本 | 32×32 Logo 底 `#2E3238` + [`icon-semi-logo.svg`](../assets/icons/icon-semi-logo.svg) 22×22 白字标 + gap 12px + 名称（Figma `1878:114807` · `1446:90540`） |
| 图片+双文本 | 同上 + 副文案 12px 45% 色 |
| StatesTag | 列表态，见 §2.8 |
| Tag | Small，见 §2.8 |

默认行高 **64px**；padding **16px 12px**；内容多时行高自适应。

#### 2.7.3 操作列 — 横向操作（默认）

```
操作一 —24px— 操作二 —24px— 操作三 —24px— 更多 [`icon-action-more.svg`](../assets/icons/icon-action-more.svg)（16×20，`#1C5CFB` 三点，Figma `1442:89933`）
```

| 类型 | 样式 |
|------|------|
| 常规操作 | 14px Medium `#1C5CFB` |
| 禁用操作 | Semibold 14px `#888D92` |
| 删除 | Semibold 14px `#F93920` |

含 Switch 见规范板 `2185:84101`；纵向操作见 `2185:84104`（gap 12px）。

### 2.8 footer / Pagination

高 **32px**；左统计 14px 60% —「显示第 1 条-第 10 条，共 46 条」；右分页 32×32，选中 `#EDF3FF` / `#1C5CFB`；距表格 **16px**。

### 2.9 列表容器

| 变体 | 容器 | 结构 |
|------|------|------|
| 单个能力 | `list-panel` | 工具栏 → 表格 → footer |
| 合集能力 | `info` | 工具栏 → 表格 → footer |
| 筛选外置 | `filter-panel` + `info` | 筛选卡；info：CardTittle → 表格 → footer |

白底、圆角 12px、`padding: 24px`、`gap: 16px`。

### 2.10 标签规范（Tag / StatesTag）`2186:90908`

#### 使用场景

| 组件 | 适用位置 |
|------|----------|
| **Tag** | 详情页、表格「标签」列（Small） |
| **StatesTag** | 表格「任务状态」列（圆点 + 文案） |

#### 状态枚举（五态）

| state | Tag 文案 | StatesTag 文案 |
|-------|----------|----------------|
| `Pending` | 待做 | 待处理 |
| `underway` | 审核 | 审核中 |
| `succeed` | 成功 | 成功 / 已完成 |
| `Effective` | 进行 | 生效中 |
| `Failed` | 失败 | 失败 |

#### Tag Small 色值（`showicon=off`）

| state | 背景 | 文字 |
|-------|------|------|
| succeed | `rgba(59,179,70,0.15)` | `#178323` |
| （其余见规范板） | — | — |

#### StatesTag 圆点色

| state | 圆点色 |
|-------|--------|
| succeed | `#3BB346` |
| Effective | `#1C5CFB` |
| Failed | `#F93920` |

#### 选型规则

| 场景 | 推荐 |
|------|------|
| 表格「任务状态」列 | `StatesTag`，`showicon=off` |
| 表格「标签」列 | `Tag` **Small** + `state=succeed` |
| 详情页状态字段 | `Tag` **Large** |

---

## 3. 布局规则

### 3.1 整页结构（按变体）

**单个能力**

```
Header → 面包屑 → intro → Line Tabs → list-panel（筛选+表格+分页）
```

**合集能力**

```
Header → 面包屑 → intro → Line Tabs → Button Tabs → info（筛选+表格+分页）
```

**筛选外置**

```
Header → 功能标题 → Line Tabs → Button Tabs → filter-panel → info（CardTittle+表格+分页）
```

### 3.2 筛选折行

`flex-wrap`；组间 24px；查询/重置 **不可拆分**；外置筛选查询+重置为同一 flex 子项（宽 160px）。

### 3.3 表格横向

总宽 = 列宽之和；容器不足时 `overflow-x: auto`。

### 3.4 Tab 联动

- **Line Tab**：切换子能力/能力时重置或加载对应数据（按业务）
- **Button Tab**：仅列表 Tab（合集默认「能力管理」、外置默认「全部」）展示列表区

---

## 4. 内容模版（Copy Slots）

| 字段 | 占位 |
|------|------|
| 能力标题 / 描述 | 能力标题 / 主标题能力描述… |
| 功能标题（外置） | 功能标题 |
| 适用 App / 链接 | 抖音、抖音极速版 / 能力接入指引 |
| Line Tab | 见 §0.3–§0.5 |
| Button Tab | 见 §0.4–§0.5 |
| 搜索 | 搜索名称 |
| 筛选 | 筛选标题 / 请选择 |
| 时间快捷 | 昨日 / 近7天 / 近30天 |
| 按钮 | 新建能力 / 查询 / 重置 |
| 列表标题 | 功能列表 |
| 表头 | 标题 / 文本信息 / 任务状态 / 标签 / 创建日期 / 操作 |
| 任务状态 | StatesTag · succeed ·「已完成」 |
| 标签列 | Tag Small · succeed ·「Tag」 |
| 分页 | 显示第 1 条-第 10 条，共 46 条 |

---

## 5. 模版配置（`LIST_PAGE_CONFIG`）

```yaml
# 自动选型（推荐）
tabLevels: 1 | 2 | 3          # ≥2 → collection
filterPlacement: inside | outside  # outside → filter-outside

# 或显式指定
variant: single | collection | filter-outside

layout:
  doc: LayoutSpec.md
  content_max_width: 1072

page:
  breadcrumb: true              # filter-outside: false
  intro: true                   # filter-outside: false
  introMeta: true               # single only
  title: "能力标题"
  functionTitle: "功能标题"     # filter-outside
  description: "..."

tabs:
  line: [能力介绍, 功能二, 功能三, 功能四]           # single
  lineCollection: [子能力一, 子能力二, 子能力三, 子能力四]
  lineFilterOutside: [能力一, 能力二, 能力三, 能力四]
  buttonCollection: [能力介绍, 能力管理, 功能三, 功能四]
  buttonFilterOutside: [全部, 能力介绍, 能力管理]
  lineDefault: 0
  buttonDefault: 1              # collection: 能力管理

filter:
  mode: simple | standard | multi-row
  placement: inside | outside
  items: [search, select, dateRange, radio+dateRange]
  actions:
    primary: { label: "新建能力", position: right }  # inside
    query: { label: 查询, width: 74 }                # outside / standard
    reset: { label: 重置, width: 74 }

list:
  container: list-panel | info
  cardTitle: 功能列表           # filter-outside only
  toolbar: true | false

table:
  columns: six | nine
  row_height: 64 | auto
  operation_layout: horizontal | vertical
  action_col_width: 264 | 248

tags:
  task_status: { component: StatesTag, state: succeed, label: "已完成" }
  label_col: { component: Tag, size: small, state: succeed, label: "Tag" }

pagination:
  page_size: 10
  show_total: true
```

---

## 6. Semi 组件映射

| 设计 | Semi |
|------|------|
| Breadcrumb / Tabs | `Breadcrumb` / `Tabs type="line"` / `Tabs type="button"` |
| 筛选 | `Input` / `Select` / `DatePicker` / `RadioGroup` / `Button` |
| 表格 | `Table` |
| Switch / Tag / StatesTag | `Switch` / `Tag` / 自定义圆点+文案 |
| 分页 | `Pagination` |

文档：[Table](https://semi.bytedance.net/zh-CN/components/table) · [Pagination](https://semi.bytedance.net/zh-CN/components/pagination) · [Radio](https://semi.bytedance.net/zh-CN/components/radio)

---

## 7. 实现与 Agent 调用

```text
生成「信息查询 / 能力列表」页：
1. LayoutSpec.md §八 + preview/LayoutSpec.html → 复制壳层（Header / 侧栏）
2. assets/README.md §3.2 → 引入 intro 元信息与表格 Logo/更多图标
3. layout-shell.css → 引入（勿在业务 CSS 覆盖 .content-area padding-top）
4. 本文件 §0 → resolveListVariant → 选定变体章节
5. preview/InfoQuery.html → 仅在 .content-area > .main 内 applyListPageConfig(LIST_PAGE_CONFIG)
6. 表格行：标题列 `TABLE_LOGO_SVG`（32×32 底 `#2E3238` + 22×22 白 Logo）· 操作列 `ACTION_MORE_SVG`（16×20 蓝三点）；金样用内联 SVG 或 UTF-8 的 `<img>`，与 Figma `1878:114807` 一致
7. SVG 资产须 UTF-8（见 assets/README.md §2.3）；勿以 Latin-1 保存含 `·` `×` 的注释，否则浏览器报 `Encoding error`、图标空白
```

**决策示例**

```js
// 单个能力列表（默认）
applyListPageConfig({ tabLevels: 1, filterPlacement: "inside" });

// 合集能力列表
applyListPageConfig({ tabLevels: 2 });

// 筛选指标在列表外
applyListPageConfig({ filterPlacement: "outside" });
```

**禁止项**

- 筛选外置时把筛选写入 `info` 或 `list-panel`
- 合集 / 外置页在 intro 展示元信息（仅单个能力 intro 有）
- 列表「任务状态」列用 Large Tag 替代 StatesTag
- 混用 [CapabilityIntro.md](./CapabilityIntro.md) 的能力价值 / 应用示例结构

---

## 8. 参考链接

- Figma 单个能力列表：[1653:120277](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=1653-120277)
- Figma 合集能力列表：[1634:68043](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=1634-68043)
- Figma 筛选外置：[1665:217264](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=1665-217264)
- Figma 表格筛选：[2185:84044](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2185-84044)
- Figma 标签规则：[2186:90908](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2186-90908)
- [assets/README.md](../assets/README.md) · [LayoutSpec.md](./LayoutSpec.md) · [CapabilityIntro.md](./CapabilityIntro.md)

---

*Figma MCP：1653:120277 · 1634:68043 · 1665:217264 · 2185:84044 · 2186:90908 及子节点*
