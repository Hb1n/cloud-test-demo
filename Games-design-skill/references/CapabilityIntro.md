# Introduction — 能力介绍页面模版

> **设计来源**：[互动游戏_平台产品-UI-Kit](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA)  
> **设计体系**：Semi Design（抖音开放平台 B 端）  
> **页面用途**：平台能力的「功能介绍」落地页模版，展示能力概览、Tab 切换、能力价值、文字说明与应用示例。  
> **HTML 预览**：[`preview/CapabilityIntro.html`](../preview/CapabilityIntro.html)（`applyPageTemplate` · `PAGE_TEMPLATE_CONFIG` · `applyPageContentConfig`）  
> 生成页面时 **必读** [LayoutSpec.md](./LayoutSpec.md) §八 + [preview/LayoutSpec.html](../preview/LayoutSpec.html)（壳层）+ [assets/README.md](../assets/README.md) §3.1 + 本文件。

---

## 0. 模版选型

生成能力介绍页前，先判断 **Tab 切换层级**（页面上纵向叠加的 Tab 导航栏数量，不含面包屑与侧栏）：

| Tab 层级 | 判定 | 选用变体 | Figma 节点 | HTML 预览 |
|----------|------|----------|------------|-----------|
| **1 层** | 仅功能切换：单层 **Line Tabs**（能力介绍 / 功能二～四） | **单个能力介绍** | `1878:102218` | `PAGE_TEMPLATE_CONFIG.tabLevels = 1` |
| **≥ 2 层** | 子能力 + 功能等 **双层 Tab**（Line 子能力 + Button 功能） | **合集能力介绍** | `1886:87371` | `PAGE_TEMPLATE_CONFIG.tabLevels ≥ 2` |

**选型规则**：

1. **仅 1 层 Tab**（单层 Line Tabs）→ **单个能力介绍**样式  
2. **≥ 2 层 Tab**（超过单层，如子能力 Line + 功能 Button 双层）→ **合集能力介绍**样式  
3. **超过 2 级 Tab**（3 层及以上）：画板暂无独立模版，**默认沿用合集能力介绍的双层结构**（最外层 Line、内层 Button，更深层级按同类型类推）

### 0.1 变体差异速查

| 对比项 | 单个能力 `1878:102218` | 合集能力 `1886:87371` |
|--------|------------------------|------------------------|
| Tab 结构 | **单层** Line Tabs | **双层**：Line（子能力）+ Button（功能） |
| intro 元信息 | 适用 App + 能力接入指引 **在 intro 底部** | 在 **info 子能力标题区** |
| info 首区块 | 能力价值标题与描述同区，**无**分割线 | 子能力标题 + 分割线 + 能力价值 |
| 主 CTA「开始接入」 | 与「能力价值」标题行右侧 | 与子能力标题区右侧 |
| 应用示例布局 | 1～3 项左对齐 `gap` 52px；4 项 `justify-between` | 4 列 `justify-between` |
| 应用示例分页 | 设计稿 **无分页**（固定 4 列）；>4 项可参考合集加分页 | **有 Pagination**（>4 项） |

### 0.2 页面信息架构

**共用（两变体）**

| 层级 | 模块 | 说明 |
|------|------|------|
| L0 | 全局壳层 | 顶栏 Header（60px）+ 左侧导航「导航max」（272px）+ 主内容区 |
| L1 | 面包屑 | 一级页面 / 二级页面 |
| L2 | 能力概览 `intro` | 能力标题 + 主描述（白卡片） |

**单个能力 · 1 层 Tab**

| 层级 | 模块 | 说明 |
|------|------|------|
| L3 | 功能切换 | **单层** Line Tabs（能力介绍 / 功能二～四） |
| L4 | 详情区 `info` | 能力价值（含描述与指标卡）→ 文字描述 → 应用示例 |

**合集能力 · ≥ 2 层 Tab**

| 层级 | 模块 | 说明 |
|------|------|------|
| L3 | 子能力切换 | Line Tabs（子能力一～四） |
| L4 | 功能切换 | Button Tabs（能力介绍 / 功能二～四） |
| L5 | 详情区 `info` | 子能力标题区 → 能力价值 → 文字描述 → 应用示例 |

---

## 1. Design Tokens

两变体共用 Semi 品牌体系；差异项在表中标注。

### 1.1 色彩（Color）

#### 品牌与语义

| Token | 值 | 用途 |
|-------|-----|------|
| `--semi-color-primary` / `palette/brand/--semi-brand-5` | `#1C5CFB` | 主按钮、激活 Tab、链接、控制台文案 |
| `palette/brand/--semi-brand-0` | `#EDF3FF` | **合集**：Button Tab 激活背景 |
| `usage/primary-light/--color-primary-light-default` | `#EAF5FF` | 导航子项选中背景 |
| `palette/blue/--semi-blue-0` | `#EAF5FF` | 浅蓝辅助背景 |
| `palette/blue/--semi-blue-1` | `#CBE7FE` | 浅蓝辅助、指标图标渐变 |
| `palette/grey/--semi-grey-5` / `usage/tertiary` | `#6B7075` | 三级/弱化操作色 |
| `Light/color/red/--red-5` | `#F93920` | 通知角标、危险提示 |
| `color/datepicker_date_selected-bg-default` | `#1c5cfb` | 日期选中等（设计变量） |

#### 文本

| Token | 值 | 用途 |
|-------|-----|------|
| `usage/text/--semi-color-text-0` | `#1C1F23` | 主标题、正文强调、面包屑当前项、激活 Tab |
| `usage/text/--semi-color-text-2` | `rgba(28, 31, 35, 0.6)` / `#1c1f2399` | 描述正文、未激活 Tab、面包屑默认项 |
| `Light/usage/text/--color-text-0` | `#171A1C` | 侧栏导航文案 |
| `usage/disabled/--semi-color-disabled-text` | `rgba(28, 31, 35, 0.35)` | 禁用文字（合集） |
| `color/breadcrumb_default-text-default` | `#1c1f2399` | 面包屑默认项 |
| `color/breadcrumb_active-text-default` | `#1c1f23` | 面包屑当前项 |
| `Text/ConstTextInverse` | `#FFFFFF` | 主按钮文字、角标文字 |

#### 背景与边框

| Token | 值 | 用途 |
|-------|-----|------|
| `Light/color/grey/--grey-0` | `#F9F9F9` | 主内容区底层背景 `bg` |
| `Light/color/white/--white` | `#FFFFFF` | 卡片、顶栏、侧栏、内容面板 |
| `usage/border/--color-border` | `#E6E9EA` | **合集**：通用分割线 |
| `color/tabs_tab_line_default-border-default` | `rgba(28, 31, 35, 0.08)` | Line Tab 底部分割线 |
| 能力价值卡片边框 | `#EDECEC` | 指标卡片描边 |
| 示例媒体占位边框 | `rgba(0, 0, 0, 0.08)` · 0.5px | 示例图/视频容器 |

#### 示例区占位色

| 用途 | 值 |
|------|-----|
| 媒体占位背景 | `rgba(100, 28, 255, 0.15)` |
| 占位提示文字 | `rgba(100, 28, 255, 0.6)` · 17px Medium |

---

### 1.2 字体（Typography）

**字体族**：`PingFang SC`（中文 UI）；数值展示 `Douyin Sans Bold` + `Douyin Number Condensed Medium`；**合集**分页数字 `Inter Regular`。

| 样式 Token | 字号 | 字重 | 行高 | 用途 |
|------------|------|------|------|------|
| `Header/header-3/CN-Semi Bold` | 24px | 600 | 32px | 能力标题（intro） |
| `Header/header-5/CN-Semi Bold` | 18px | 600 | 24px | 区块标题（能力价值、文字描述、应用示例；合集含子能力标题） |
| `Paragraph/regular/CN-Semi Bold` | 14px | 600 | 20px | 激活 Tab、主按钮、示例小标题 |
| `Paragraph/regular/CN-Regular` | 14px | 400 | 20px | 正文、面包屑、导航、能力价值描述 |
| `Paragraph/small/CN-Regular` | 12px | 400 | 16px | 示例描述（最多两行）；合集 AppID 副文案 |
| `CN/14_P1_Medium` | 14px | 500 | 100% | 链接「能力接入指引」 |
| 指标数字 | 24px | Bold | 36px | 能力价值百分比主数字 |
| 指标单位 `%` | 14px | Medium | — | 紧跟数字 |

---

### 1.3 间距（Spacing）

| Token / 场景 | 值 |
|--------------|-----|
| `spacing/breadcrumb_item-marginRight` | 4px |
| `spacing/breadcrumb_item_wrap-marginRight` | 4px |
| `spacing/tabs_bar_line_tab-paddingTop` | 16px |
| `spacing/tabs_bar_line_tab-paddingBottom` | 14px |
| `spacing/tabs_bar_line_tab-marginRight` | 24px |
| `spacing/tabs_content-paddingY` | 5px（设计变量）；Tabs 组件底部预留 **10px** 内容区 |
| `spacing/tabs_bar_button_tab-paddingX` | 12px（**合集** Button Tab） |
| `spacing/tabs_bar_button_tab-paddingY` | 8px（**合集**） |
| `spacing/tabs_bar_button_tab-marginRight` | 8px（**合集**） |
| `spacing/pagination_item-marginLeft/Right` | 4px（**合集**） |
| 主框架 `pt` / Header 底 → 内容区顶 | **32px**（`content-area` 上内边距） |
| 面包屑 → intro | 16px |
| intro → Tabs 区 | 8px |
| Line Tabs → Button Tabs | 16px（**合集**） |
| Tabs → info 顶 | 16px（含 51px Tab 栏 + 间距） |
| intro / info 卡片 `padding` | 24px |
| intro 内部 `gap` | 16px（标题 / 描述 / 元信息） |
| info 内部区块 `gap` | 32px（大段之间） |
| 能力价值首块内部 `gap` | 16px（标题区与双卡指标） |
| 标题与描述（区块内） | 16px |
| 描述列表行 `gap` | 8px |
| 元信息：图标与标签 | 4px（指引链接）；适用 App 标签与值 8px |
| 元信息两项间距 | 24px |
| 能力价值双卡 `gap` | 16px |
| 示例列 `gap`（标题区与图） | 16px；标题与副文案 6px |
| 示例列间距（单个 · 1～3 项） | **52px**（左对齐，`flex` + `gap`） |
| 示例列间距（单个 · 4 项） | 约 53px（`justify-between` 均分） |

---

### 1.4 圆角（Radius）

| Token | 值 | 用途 |
|-------|-----|------|
| `radius/tabs_tab_button` | 3px | **合集** Button Tab |
| `radius/pagination_item` | 3px | **合集**分页按钮 |
| 主按钮 | 3px | 「开始接入」 |
| intro / info 卡片 | 12px | 内容白卡片 |
| 能力价值卡、示例媒体 | 12px | 子卡片 |
| 侧栏应用头像 | 6px | Logo 区域 |
| 通知角标 | 8px | Num 99+ |

---

### 1.5 尺寸（Sizing）

| 元素 | 尺寸 |
|------|------|
| 画板总宽 | 1440px |
| 顶栏 Header | 高 60px，全宽 |
| 侧栏「导航max」 | 宽 272px |
| 主内容 `bg` | 宽 1168px |
| 内容框架 `框架` | 宽 1072px，左偏移 x=320 或 `calc(20% + 32px)` |
| intro 卡片 | 单个示例高 152px，内宽 1008px |
| info 卡片 | 内宽 1024px（padding 后内容区） |
| 主按钮 | 80×32px 或 py 6px + px 12px |
| 分页项（合集） | 高 32px，min-width 32px；页码区 min-width 44px |
| 指标图标 | 48×48px |
| 指标卡片（单个） | 高 90px，单列宽约 496px（2 列均分） |
| 示例单列 | 宽 216px，媒体高 467px |
| 列表圆点 | 5×5px |
| 图标（元信息） | 16×16px |

---

## 2. 组件规范（Components）

### 2.1 共用：Header（顶栏）

1440×60px，白底，阴影 `0 2px 2px rgba(0, 0, 0, 0.03)`；Logo · 顶导 · 搜索（200px）· 控制台 `#1C5CFB` · 用户区。顶导 14px Regular，项间距 24px。

### 2.2 共用：导航max（侧栏）

272px 宽，应用信息 + 一级/二级 NavigationItem；选中背景 `#EAF5FF`。应用头像 40×40，圆角 6px。

### 2.3 共用：Breadcrumb（面包屑）

- **高度**：28px
- **默认项**：`color/breadcrumb_default-text-default` → 60% 透明度
- **当前项**：`#1C1F23`
- **分隔符**：`/`

### 2.4 共用：Button — Primary「开始接入」

- **文档**：[Semi Button](https://semi.bytedance.net/zh-CN/components/button)
- **背景**：`#1C5CFB`；文字 14px Semibold `#FFFFFF`
- **内边距**：px 12px，py 6px；圆角 3px
- **位置**：单个 — 与「能力价值」标题行右侧；合集 — 与子能力标题区右侧（flex justify-end）

### 2.5 共用：能力价值 — 指标卡片

- **布局**：2 列等分（flex 1），列间距 16px
- **卡片**：边框 1px `#EDECEC`，圆角 12px，padding 16px，高约 90px
- **内容**：左 `metric-value-rise.svg`（48×48）趋势图标 + 右栏数字与说明
- **数字**：示例 `10` / `25` + `%`；说明 14px Regular 60% 色

### 2.6 共用：文字描述 — 列表

- **区块标题**：18px Semibold
- **列表项**：5px 圆点 + 8px gap + 14px Regular 正文（60% 色）
- **行间距**：8px

### 2.7 共用：应用示例 — 媒体卡片列

- **单列**：宽 216px；列内纵向 `gap` 16px（标题区 → 媒体）
- **标题**：14px Semibold
- **描述**：12px Regular，最多两行（16px 行高），标题区总高 58px
- **媒体区**：216×467px，圆角 12px，占位「支持图片、视频」

### 2.8 单个能力变体

#### intro（能力概览卡片）

- **容器**：白底、`border-radius: 12px`、`padding: 24px`，纵向 `gap: 16px`
- **能力标题**：24px Semibold，`#1C1F23`
- **主描述**：14px Regular，`rgba(28,31,35,0.6)`
- **元信息行**（位于 intro 底部，**非** info 内）：
  - **适用 App**：`icon-mobile-app.svg`（16×16）+「适用 App :」+ 值，14px Regular 60% 色
  - **能力接入指引**：`icon-brief-stroked.svg`（16×16）+ 14px Medium `#1C5CFB`，间距 24px
- **流程描述**（三列 step）：默认 **hidden**

#### Tabs — Line（功能切换，唯一 Tab 层）

- **Tab 项**：能力介绍（默认激活）、功能二、功能三、功能四
- **类型**：underline line tabs，栏高 51px
- **激活**：底边 2px `#1C5CFB`，14px Semibold `#1C1F23`
- **未激活**：透明底边，14px Regular 60% 色
- **Tab 间距**：24px

> **注意**：单个能力模版**不使用** Button Tabs，也**无**子能力 Line Tabs。

#### info（详情白卡片）

- **容器**：白底、圆角 12px、padding 24px，内部区块纵向 gap **32px**
- **首块**：能力价值标题 + 描述 + CTA，**无**子能力标题、**无**分割线、**无** info 内元信息
- **应用示例布局**：
  - **1～3 项**：左对齐，`flex` + `gap: 52px`
  - **4 项**：`justify-content: space-between`
- **分页**：设计稿**不包含** Pagination；>4 项时可参考合集加分页

### 2.9 合集能力变体

#### intro（能力概览卡片）

- 白底、圆角 12px、padding 24px；仅能力标题 + 主描述
- **元信息不在 intro**，位于 info 子能力标题区

#### Tabs — Line（子能力切换）

- underline line tabs，栏高 51px；激活底边 2px `#1C5CFB`；Tab 间距 24px

#### Tabs — Button（功能介绍 / 功能二…）

- **激活**：背景 `#EDF3FF`，文字 `#1C5CFB` 14px Semibold，圆角 3px
- **未激活**：透明底，文字 60% 透明度 14px Regular
- **内边距**：12px × 8px；项间距 8px
- **与 Line Tabs 间距**：约 16px

#### info（详情白卡片）

- 首个区块：**子能力标题区**（18px Semibold）+ 元信息行 + CTA + **分割线**（1px `#E6E9EA`）→ 能力价值
- **元信息行**：适用 App + 能力接入指引（同单个，位置在 info 内）
- **应用示例**：4 列 `justify-between`；底部分页居中

#### Pagination（分页）

- **位置**：示例区底部居中；高 32px
- **样式**：前后翻页 + `当前/总数`（如 2/2）；圆角 3px

#### Tag（流程描述内，默认隐藏）

三步流程卡片（328×72）在 intro 内为 **hidden**，扩展时启用。

---

## 3. 布局规则（Layout Rules）

### 3.1 栅格与区域划分

**单个能力（1 层 Tab）**

```
┌────────────────────────────────────────────────────────────── 1440px ──┐
│ Header 60px                                                            │
├──────────┬─────────────────────────────────────────────────────────────┤
│ 侧栏     │  bg #F9F9F9                                                 │
│ 272px    │  ┌─ 框架 1072px (padding-top 32px) ─────────────────────┐  │
│          │  │ Breadcrumb                                             │  │
│          │  │ ┌─ intro, r12, p24, gap16 ───────────────────────────┐ │  │
│          │  │ │ 能力标题 · 描述 · 适用App+接入指引（intro 内）     │ │  │
│          │  │ └────────────────────────────────────────────────────┘ │  │
│          │  │ Line Tabs（能力介绍/功能二～四）→ info 卡片            │  │
│          │  └────────────────────────────────────────────────────────┘  │
└──────────┴─────────────────────────────────────────────────────────────┘
```

**合集能力（≥ 2 层 Tab）**

```
┌────────────────────────────────────────────────────────────── 1440px ──┐
│ Header 60px                                                            │
├──────────┬─────────────────────────────────────────────────────────────┤
│ 侧栏     │  bg #F9F9F9                                                 │
│ 272px    │  ┌─ 框架 1072px (padding-top 32px) ─────────────────────┐  │
│          │  │ Breadcrumb                                             │  │
│          │  │ ┌─ intro 卡片, r12, p24 ─────────────────────────────┐ │  │
│          │  │ │ 能力标题 24px · 描述 14px                          │ │  │
│          │  │ └────────────────────────────────────────────────────┘ │  │
│          │  │ Line Tabs → Button Tabs → info 卡片                     │  │
│          │  └────────────────────────────────────────────────────────┘  │
└──────────┴─────────────────────────────────────────────────────────────┘
```

### 3.2 对齐规则

| 规则 | 说明 |
|------|------|
| 主内容左对齐 | 框架、卡片、标题均左对齐 |
| 主操作右对齐 | 「开始接入」与标题行右侧 |
| 分页居中 | **合集**：应用示例底部分页水平居中 |
| 双列指标 | 能力价值区 50% + 50%，等高卡片 |
| 应用示例列 | 等宽 216px；单个 1～3 项 `gap` 52px，4 项或合集 4 列 `justify-between` |

### 3.3 垂直节奏

| 区间 | 间距 |
|------|------|
| Header 底 → 主内容区顶 | 32px |
| 面包屑 → intro | 16px |
| intro → Line Tabs | 8px |
| Line Tabs → Button Tabs | 16px（合集） |
| Line Tabs 栏 → info 顶 | 16px |
| info：能力价值块 → 文字描述 → 应用示例 | 各 32px |
| 能力价值：标题区 → 指标卡 | 16px |
| 区块内标题 → 列表/示例 | 16px |
| 应用示例标题区 → 媒体 | 16px |

### 3.4 卡片容器规则

- **intro**：无描边，靠 `#F9F9F9` 底衬托；单个变体元信息必须在 intro 内完成
- **info**：多个逻辑区块用 32px 分隔；合集首块含标题+操作+元信息+分割线

### 3.5 响应式说明（设计稿基准）

- 固定宽度 **1440px**；内容区 min-width 建议 ≥ 1072px
- 侧栏折叠行为两变体一致（设计稿未展开折叠态）

---

## 4. 内容模版（Copy Slots）

| 字段 | 单个能力占位 | 合集能力占位 |
|------|--------------|--------------|
| 能力标题 | 能力标题 | 能力标题 |
| 能力描述 | 主标题能力描述内容… | 主标题能力描述内容… |
| 子能力 Tab | — | 子能力一 / 二 / 三 / 四 |
| 功能 Tab | 能力介绍（默认）/ 功能二～四 | 能力介绍（默认激活）/ 功能二～四 |
| 子能力标题 | — | 子能力标题 |
| 适用 App | 抖音、抖音极速版 | 抖音、抖音极速版 |
| 链接 | 能力接入指引 | 能力接入指引 |
| 能力价值标题 | 能力价值 | — |
| 能力价值描述 | 段落描述… | — |
| CTA | 开始接入 | 开始接入 |
| 能力价值指标 | 10% 次日留存；25% 7日留存 | 10% 次日留存率平均提升；25% 7日留存率平均提升 |
| 文字描述列表 | 第一条/第二条描述内容 | 第一条/第二条描述内容 |
| 应用示例 | 标题 1～4；描述最多两行 | 标题 5～8；描述最多两行；媒体占位 |

---

## 5. 模版配置（info 模块可展示 / 可隐藏）

通过 `PAGE_CONTENT_CONFIG`（见 [`preview/CapabilityIntro.html`](../preview/CapabilityIntro.html)）控制 **能力价值 / 文字描述 / 应用示例** 三块。**默认三项均展示**，与画板一致。合集变体 info 结构不同，但三块显隐逻辑相同。

### 5.1 配置结构

```yaml
page_content:
  capabilityValue:
    show: true
    showTitle: true
    showIntro: true
    showMetrics: true
    showCta: true
    title: "能力价值"
    intro: "段落描述…"
    ctaText: "开始接入"
    metrics:              # showMetrics=true 时至少 2 项、最多 4 项
      - { value: "10", unit: "%", label: "次日留存率平均提升" }
  textDescription:
    show: true
    title: "文字描述"
    items: ["第一条…", "第二条…"]
  applicationExamples:
    show: true
    title: "应用示例"
    items:
      - { title: "标题 1", desc: "最多两行…", mediaSrc: "", mediaType: "image" }
```

### 5.2 展示判定（约束）

| 模块 | 展示条件 | 不展示时 |
|------|----------|----------|
| 能力价值 | `show: true` 且标题/描述/CTA/有效指标 **至少一项** 可展示 | `.info-section` 加 `is-hidden`，不占位 |
| 能力价值·指标卡 | `showMetrics: true` 且 `metrics` ≥ 2 | 仅隐藏 `#metrics-grid`，标题区可保留 |
| 文字描述 | `show: true` 且 `items.length > 0` | 整段隐藏 |
| 应用示例 | `show: true` 且 `items.length > 0` | 整段隐藏；分页器一并隐藏 |
| 三块全隐 | — | `.card.info.is-content-empty` 隐藏白卡片 |

> `show: true` 但数据不满足时**视同不展示**，避免空标题悬空。intro 卡片（能力标题、元信息）**不受** 上述三块 `show` 影响。

### 5.3 不展示时的布局

| 场景 | 行为 |
|------|------|
| 部分模块隐藏 | `#info-sections` 使用 `flex` + `gap: 32px`，仅可见块参与间距 |
| 三块全部隐藏 | `.card.info` 加 `is-content-empty` 并 `display: none` |
| 能力价值无指标卡 | 标题与 CTA 间距保持；指标区 `margin-top` 归零 |
| intro 段落为空 | `showIntro: false` 或 `intro` 留空时隐藏描述段落 |

### 5.4 调用

```javascript
applyPageContentConfig(PAGE_CONTENT_CONFIG);
```

---

## 6. 模版配置（`PAGE_TEMPLATE_CONFIG`）

**适用**：[preview/CapabilityIntro.html](../preview/CapabilityIntro.html)（`applyPageTemplate`）

| 配置项 | 说明 |
|--------|------|
| `tabLevels` | `1` → 单个能力样式；`≥ 2` → 合集能力样式 |
| `page.title` / `page.desc` | intro 卡片标题与描述 |
| `subCapabilities[]` | ≥2 层时 Line Tabs 子能力项 |
| `functionTabs[]` | 功能 Tab 项（单层 Line 或双层 Button） |
| `subCapability.title` / `desc` | 合集 info 子能力标题区 |

```javascript
PAGE_TEMPLATE_CONFIG.tabLevels = 2; // 切换为合集样式
applyPageTemplate(PAGE_TEMPLATE_CONFIG);
```

URL 预览：`CapabilityIntro.html?tabs=1`（单个）· `?tabs=2`（合集）

---

## 7. 实现建议

- **图标**：必读 [assets/README.md](../assets/README.md) §3.1；HTML 用 `assets/icons/*.svg` 的 `<img>`（见 preview），**禁止**只写 `semi-icons-*` 名称
- **组件库**：`@douyinfe/semi-ui` — `Breadcrumb`、`Tabs`（`type="line"`；合集另加 `type="button"`）、`Button`、`Pagination`（合集）、`Typography`
- **CSS 变量**：品牌色 `#1C5CFB`；勿在单个能力页照搬合集双层 Tabs
- **页面路由**：运营能力等侧栏菜单下的能力详情/功能介绍页
- **选型**：先 `applyPageTemplate` 按 Tab 层级选变体，再 `applyPageContentConfig` 控制 info 三块
- **单个 · 应用示例**：`#examples-grid` 按 `data-count`（1～4）切换布局
- **合集 · 应用示例**：>4 项启用 Pagination
- **扩展**：intro 内「流程描述」三列 step 默认隐藏

---

## 8. 参考链接

- Figma：[单个能力页面模版 node 1878:102218](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=1878-102218)
- Figma：[应用示例（1～3 项左对齐）node 2224:32175](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2224-32175)
- Figma：[合集能力页面模版 node 1886:87371](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=1886-87371)
- [assets/README.md](../assets/README.md) · [LayoutSpec.md](./LayoutSpec.md)
