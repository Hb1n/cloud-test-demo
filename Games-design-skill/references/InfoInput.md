# Form — 信息输入

> **设计来源**：[互动游戏_平台产品-UI-Kit](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit)  
> - **单一信息模块** `1732:122528`（未填写）· 左卡 `1732:122536` · 预览 `2173:26619` · 页脚 `1816:118365`  
> - **分层信息模块** [`2286:31417`](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2286-31417)（未填写）· 左卡 `2286:31547` · 预览 `2286:33426` · 页脚 `2286:31956`  
> **设计体系**：Semi Design（抖音开放平台 B 端）  
> **页面用途**：能力配置下的**信息录入页**（单一 / 分标题两种结构）：面包屑 + 表单左栏 + 实时预览右栏 + 底部固定提交栏。生成页面时 **必读** [LayoutSpec.md](./LayoutSpec.md) §八 + [preview/LayoutSpec.html](../preview/LayoutSpec.html)（壳层）+ [assets/README.md](../assets/README.md) §3.3 + 本文件。  
> **HTML 预览**：[`preview/InfoInput.html`](../preview/InfoInput.html)（`applyFormFieldConfig` · `FORM_FIELD_CONFIG`）  
> **模版契约**：见 §8（源自 `info-entry.skill/template.yaml`）  
> **更新日期**：2026-06-16

---

## 0. 页面信息架构

| 层级 | 模块 | 说明 |
|------|------|------|
| L0 | 全局壳层 | 顶栏 Header（60px）+ 侧栏「导航max」（272px）+ 主内容区 `#F9F9F9`（见布局规范） |
| L1 | 面包屑 | 多级路径（画板示例：一级 / 二级 / 三级 / 四级） |
| L2 | **表单区** `form-row` | 左 **表单卡片** + 右 **预览卡片**，水平间距 **24px** |
| L3 | 页脚 `form-footer` | **固定底栏** SideSheetFooter：取消 + 提交 |
| L4（分层） | **填写分区** `填写` × N | 仅 `2286:31417`：头部分割线后多区，每区 **信息标题** + 字段组 |

### 两种变体

| 变体 | Figma 节点 | 填写区结构 |
|------|------------|------------|
| **单一信息模块** | `1732:122528` | 分割线后 **一整块** `form-body`，14 项连续 |
| **分层信息模块** | `2286:31417` | 分割线后 **多个** `form-section`，每区含「信息标题」 |

### 适用场景（预览 / 信息块）

| 场景 | `preview.enabled` | 信息块标题 |
|------|-------------------|------------|
| 配置项可实时预览（素材、文案等） | 开，预览 Tab **1～3** 个 | 多块时显示「信息标题」 |
| 纯配置 / 列表填报 | **关**，表单白卡全宽 | 单块不显示「信息标题」 |

> 预览 Tab **仅 1 个时不渲染**右上角分段控件；超过 3 个 Tab 禁止。

### 核心模式（共用）

| 规则 | 说明 |
|------|------|
| **白底卡片** | 左表单、右预览均为 **`#FFFFFF` 圆角 12px** 独立卡片，浮于 `#F9F9F9`；面包屑在白卡外 |
| 双栏布局 | 左表单 `718px`（flex:1）+ 右预览 `330px`；总内容宽 **1072px** |
| 无预览 | 白卡全宽；纯输入控件 **width: 100%** 填满 `.form-control-col`，最大宽 `--form-column-max-width` |
| 滚动 | 卡片区域**不单独滚动**，内容默认完全展示；页面随内容区自然滚动；预览区固定 |
| 表单内标题区 | 页面标题 + 描述 + 元信息（当前接入 / 能力接入指引），**非**独立 intro 卡片 |
| 只读信息行 | 首行「信息名称」为 **只读展示**（标签 + 文案），非输入控件 |
| 字段描述 | 控件下方 **12px** 辅助说明；错误态在控件组内展示 |
| 预览联动 | 右侧预览区含 **分段 Radio** 切换预览态 + Skeleton 占位 |
| 底栏固定 | 页脚 **80px** 全宽（侧栏右侧起），主操作右对齐 |

### 0.1 单一信息模块（`1732:122528`）

| 规则 | 说明 |
|------|------|
| 填写区 | 头部分割线后单一 `form-body`，纵向 `gap: 24px` |
| 分区标题 | **无** |
| 左卡节点 | `1732:122536` |

### 0.2 分层信息模块 · 分标题结构（`2286:31417`）

| 规则 | 说明 |
|------|------|
| **单白卡多分区** | 全部分区在同一左白卡 `2286:31547` 内，**不**为每区单独建卡 |
| **信息标题** | 每区顶部 14px Semibold「信息标题」；**不带**必填 `*` |
| **小标题模块间距** | `.form-section` 纵向 `gap: 24px`：**信息标题 → 首字段组** **24px**（画板 `2286:31571`：`gap-[24px]`，标题高 20 → 字段组 `y=44`） |
| **分区间距** | 相邻 `填写` 区块间距 **24px**（`.form-sections { gap: 24px }`；示例：首区底 `y=768` → 次区顶 `y=792`） |
| **区内字段** | `.form-body` 纵向 `gap: 24px`，控件规范与单一模块 **相同** |
| **空分区** | 区内无可见字段时，**整区隐藏**（避免空标题） |

**画板分区与字段归属**

| 分区 | 节点 | 信息标题 | 包含字段（顺序） |
|------|------|----------|------------------|
| 填写一 | `2286:31571` | 信息标题 | 信息名称 · 数字输入框 · 选择框 · 搜索框 · 输入框 · 文本输入框 |
| 填写二 | `2286:31683` | 信息标题 | 时间筛选 · 日期筛选 · 日期范围 · 示例图片（1:1）· 示例图片（16:9）· 添加按钮 · 单选框 · 复选框 |

### 0.3 变体差异（单一 vs 分层）

| 对比项 | 单一 `1732:122528` | 分层 `2286:31417` |
|--------|-------------------|-------------------|
| 表单结构 | 单块 `form-body` | 多个 `form-section` |
| 分区标题 | 无 | 每区 **信息标题** 14px Semibold |
| 字段编排 | 14 项连续 | 按业务拆区（画板示例 6+8） |
| 控件区宽 | **100%**（`.form-control-col` flex:1；画板默认约 **476px**） | **100%**（同上） |
| 预览 / 页脚 / 元信息 | — | **一致** |

### 0.4 与相关模版差异

| 对比项 | 单个能力介绍 `1878:102218` | 列表页 | **信息输入（本文件）** |
|--------|---------------------------|--------|----------------------|
| 面包屑 | 2 级 | 有 / 无 | **多级**（示例 4 级） |
| 页面标题 | intro 卡片 24px | intro 或仅标题 | **表单卡片内** 18px |
| Tabs | Line（能力介绍） | Line / Line+Button | **无** |
| 主内容 | 能力价值 + 示例 | 表格 / 看板 | **表单 + 预览** |
| 底栏 | 无 | 分页 footer | **固定提交栏** |
| 元信息 | intro 内 | 列表 intro 内 | **表单头部**内 |

---

## 1. Design Tokens

### 1.1 色彩

| Token | 值 | 用途 |
|-------|-----|------|
| `palette/brand/--semi-brand-5` | `#1C5CFB` | 主按钮（提交）、能力接入指引链接、Radio 选中、预览 Tab 激活 |
| `palette/brand/--semi-brand-0` | `#EDF3FF` | — |
| `usage/text/--semi-color-text-0` | `#1C1F23` | 标题、标签、控件已填、面包屑当前项 |
| `usage/text/--semi-color-text-1` | `#1c1f23cc` / 80% | 添加按钮文案、Radio 选中项文案 |
| `usage/text/--semi-color-text-2` | `#1c1f2399` / 60% | 描述、辅助说明、占位、面包屑默认项 |
| `Light/usage/text/--color-text-0` | `#1C1F23` | 预览标题 |
| `Light/usage/danger/--color-danger` | `#F64F4F` | 表单错误文案 |
| `Light/color/red/--red-5` | `#F93920` | 必填 `*`、错误图标 |
| `color/breadcrumb_default-text-default` | `#1c1f2399` | 面包屑链接 |
| `color/breadcrumb_active-text-default` | `#1c1f23` | 面包屑当前 |
| `color/breadcrumb_sepearator_default-icon-default` | `#1c1f2399` | 分隔符 `/` |
| `color/select-bg-default` | `rgba(46,50,56,0.05)` | Select / Input / 上传区底 |
| `color/input_default-bg-default` | `rgba(46,50,56,0.05)` | Input / TimePicker 底 |
| `color/input_disabled-bg-default` | `rgba(46,50,56,0.04)` | 未填写 / 禁用 Input、TextArea |
| `color/input_disabled-text-default` | `rgba(28,31,35,0.35)` | 禁用占位 |
| `color/select_input_placeholder-text` | `#1c1f2399` | Select 占位 |
| `color/input_counter-text-default` | `#1c1f2399` | 字数统计 `0/30` |
| `color/button_light-bg-default` | `rgba(46,50,56,0.05)` | 添加内容（Light Button） |
| `color/button_primary-bg-default` | `#1c5cfb` | 提交按钮 |
| `color/radio_primary-bg-default` | `#1c5cfb` | Radio 选中圆 |
| `color/checkbox_checked-bg-default` | `#0064FA` | Checkbox 选中底 |
| `color/skeleton_default-bg-default` | `rgba(46,50,56,0.05)` | 预览 Skeleton |
| `usage/border/--color-border` | `#E6E9EA` | 表单头部分割线 |
| `usage/border/--semi-color-border` | `rgba(28,31,35,0.08)` | 上传虚线框、InputNumber 按钮边 |
| `Light/color/grey/--grey-0` | `#F9F9F9` | 主内容区底 |
| `Light/color/white/--white` | `#FFFFFF` | 表单卡、预览卡、页脚 |
| `Text/ConstTextInverse` | `#FFFFFF` | 提交按钮字 |
| `CN/14_P1_Medium` | `#1C5CFB` | 能力接入指引（Medium 14） |

### 1.2 字体

**字体族**：`PingFang SC`（数字 / 计数可用 `Inter`）。

| 样式 | 规格 | 用途 |
|------|------|------|
| `Header/header-5/CN-Semi Bold` | 18/600/24 | 表单内「页面标题」、预览区「预览图」 |
| `Paragraph/regular/CN-Regular` | 14/400/20 | 描述、标签、控件、面包屑、元信息 |
| `Paragraph/regular/CN-Semi Bold` | 14/600/20 | **分层 · 信息标题**、添加按钮、预览分段激活项 |
| `Paragraph/small/CN-Regular` | 12/400/16 | 字段辅助说明、错误文案、预览分段未激活 |
| `Paragraph/small/CN-Semi Bold` | 12/600/16 | 预览分段激活项 |
| `CN/14_P1_Medium` | 14/500/20 | 能力接入指引链接 |

### 1.3 间距与尺寸

| 场景 | 值 |
|------|-----|
| 内容框架宽 | **1072px** |
| Header 底 → 面包屑顶 | **32px**（画板内容区 `y=92`，Header `60px`） |
| 面包屑区高度 | **56px**（面包屑 `y=32`，高 **24px**） |
| 面包屑 → 表单行 | **16px**（form `y=72`，面包屑区底 `56px`） |
| 表单行内：左卡 ↔ 预览 | **24px** |
| 左表单卡片 padding | **24px 32px** |
| 预览卡片 padding | **24px 16px** |
| 表单头（标题区）内部 gap | **16px** |
| 标题 → 描述 | **16px**（描述 `y=40`，标题高 `24px`） |
| 元信息行 gap | **24px**（两组元信息） |
| 元信息图标 ↔ 文案 | **4px** / **8px** |
| 头部分割线 → 填写区 / 首分区 | **24px**（分割线 `y=144`，填写 `y=168`） |
| **分层 · `.form-section` gap** | **24px**（信息标题 ↔ 字段组；`2286:31571`） |
| **分层 · `.form-sections` gap** | **24px**（相邻填写分区） |
| **表单项行间距** | **24px** |
| 标签列宽 | **171px**（首行只读 **170px**） |
| 标签 ↔ 控件 | **8px** |
| 控件 ↔ 辅助说明 | **8px** |
| 控件默认高度 | **32px** |
| 控件区宽 | **100%**（`.form-control-col`，随表单卡片可用宽度自适应；画板默认约 476px） |
| 纯输入控件宽 | Input / Select / TextArea / TimePicker / DatePicker：**width: 100%** |
| 上传卡片宽 | 固定比例尺寸（1:1 · 16:9），**不**随列拉伸 |
| 数字输入子行间距 | **8px**（现价行 `y=40`） |
| Radio 组水平间距 | **16px** |
| Checkbox 组水平间距 | **16px** |
| 页脚高度 | **80px** |
| 页脚内按钮区 | 右对齐；取消 **52px** + 提交 **52px`，间距 **12px** |
| 页脚内容左右 inset | 相对内容区 **80px** 左、**48px** 右（画板 `x=80`，宽 `1040`） |
| 主内容底部留白 | **≥48px**（避免被固定页脚遮挡） |

### 1.4 圆角

| 元素 | 值 |
|------|-----|
| 表单 / 预览卡片 | **12px** |
| Input / Select / Button / 上传 / 预览分段 | **3px** |
| Radio 外圈 | **16px**（圆形） |
| Checkbox | **3px** |

---

## 2. 布局规则

### 2.1 页面结构

**单一信息模块**（`1732:122528`）

```
content-area (max-width: 1072px, padding 由 layout-shell.css 32/24/24, background: #F9F9F9)
├── breadcrumb (height: 24px, margin-bottom: 16px)  ← 无白卡包裹
└── form-row (display: flex, gap: 24px, align-items: flex-start)
    ├── .card.form-card (flex: 1, bg: #FFF, radius: 12px, ~718px)
    │   ├── form-header（标题 + 描述 + 元信息）
    │   ├── divider
    │   └── form-body（纵向 gap: 24px）
    └── .card.preview-card (width: 330px, bg: #FFF, radius: 12px, flex-shrink: 0)
└── [viewport] form-footer (fixed, height: 80px)
```

**分层信息模块**（`2286:31417`）

```
content-area
├── breadcrumb
└── form-row
    ├── .card.form-card (px: 32px, pt: 24px)
    │   ├── form-header「文字部分」
    │   ├── hr.form-divider（1px #E6E9EA）
    │   └── .form-sections（纵向 gap: 24px）
    │       └── .form-section「填写」× N（纵向 gap: 24px）
    │           ├── h3.section-title「信息标题」（14px Semibold）
    │           └── .form-body（纵向 gap: 24px）
    └── .card.preview-card (330px)
└── form-footer (80px)
```

### 2.2 表单项行（`cell`）布局

```
cell (display: flex, gap: 8px, align-items: flex-start)
├── form-field (width: 171px, min-height: 32px, flex-shrink: 0)
│   └── form-label: 文案 + [help_icon] + [*]
└── field-control (flex: 1, min-width: 0, width: 100%, display: flex, flex-direction: column, gap: 8px)
    ├── 控件（Input / Select / …，纯输入类 width: 100%）
    ├── extra-hint（12px 辅助说明，可选）
    └── error-msg（12px 错误，可选）
```

| 规则 | 说明 |
|------|------|
| 标签对齐 | 标签列顶对齐；单行控件与标签 **垂直居中**（`items-center`） |
| 控件列宽 | `.form-control-col`：`flex: 1; min-width: 0; width: 100%`；纯输入类控件 **100%** 填满可用宽 |
| 多行控件 | 数字输入双行、TextArea、上传等用 `items-start` |
| 必填 | 红色 `*` 紧跟标签，左间距 **4px** |
| 帮助图标 | `icon-help-circle-16.svg` **16×16**（`rgba(23,26,28,0.45)`），标签后 **4px**（数字输入框 `1732:122579`） |
| 只读行 | 无控件，右侧直接展示文案（与标签同行居中） |

### 2.3 左表单白卡 `form-card`（`1732:122536`）

| 属性 | 值 |
|------|-----|
| 背景 | `#FFFFFF`（`Light/color/white/--white`） |
| 圆角 | **12px** |
| 内边距 | **24px 32px**（`py-24 px-32`） |
| 内部纵向 gap | **24px**（标题区 / 分割线 / 填写区） |
| 宽度 | **flex: 1**，画板约 **718px** |
| 溢出 | `overflow: hidden` |

面包屑 **不在** 本白卡内；仅标题 + 描述 + 元信息 + 表单项。

### 2.4 表单头部 `form-header`

| 元素 | 规格 |
|------|------|
| 页面标题 | 18px Semibold，`#1C1F23` |
| 描述 | 14px Regular，60% 透明度，单行或多行 |
| 元信息行 `1732:122546` | 两组水平排列，`gap: 24px`，`align-items: flex-start` |
| 元信息 · 当前接入 `1732:122547` | `icon-pulse.svg` **16×16**（`rgba(28,31,35,0.6)`，Figma `1732:122551`）+ 文案区 `gap: 8px` |
| 元信息 · 指引 `1732:123495` | `icon-brief-stroked.svg` **16×16**（`#1C5CFB`，Figma `1732:123496`）+ 「能力接入指引」14px **Medium** |
| 分割线 | 1px `#E6E9EA`，宽 100%（内容区内） |

### 2.5 预览区 `preview-card`

| 元素 | 规格 |
|------|------|
| 卡片 | 白底、圆角 12px、固定宽 **330px**、最小高 **646px** |
| 标题行 | 「预览图」18px Semibold + 右侧 **分段 RadioGroup** |
| 分段控件 | 轨道底 `rgba(46,50,56,0.05)`，高 **32px**，内 padding **4px**；激活项白底 + 品牌色字 **12px Semibold** |
| 预览画布 | Skeleton **298×530**，圆角 3px，灰底 5% |
| 画布说明 | 「预览图片标题&介绍」14px Regular，画布下 **8px** |

### 2.6 固定页脚 `form-footer`

| 属性 | 值 |
|------|------|
| 定位 | `position: fixed; bottom: 0; left: 272px; right: 0`（侧栏宽） |
| 高度 | **80px** |
| 背景 | `#FFFFFF` |
| 顶边 | 1px 分割线 |
| 内容 | `padding: 24px 48px 24px 80px`（相对壳层） |
| 按钮组 | 右对齐；**取消**（次要 52×32）+ **提交**（主色 52×32） |
| 节点 | 单一 `1816:118365` · 分层 `2227:33428` |

### 2.7 分层信息模块 · 分标题布局规则

| 规则 | 说明 |
|------|------|
| 分区容器 | `.form-section`：`display: flex; flex-direction: column; gap: 24px`；仅 **标题 + 字段组**，无独立背景/描边 |
| 信息标题 | 14px Semibold `#1C1F23`；与 `.form-body` 间距 **24px**（由 `.form-section` gap 实现） |
| 区内字段 | 复用 §2.2 `cell` 左右栏布局；`.form-body` 内行间距 **24px** |
| 分区间距 | `.form-sections { gap: 24px }` |
| 分区顺序 | 自上而下；`section.show: false` 时整区隐藏 |
| 预览对齐 | 右预览与左卡顶对齐（`align-items: flex-start`） |
| 填写分区节点 | 填写一 `2286:31571` · 填写二 `2286:31683` |
| 分割线节点 | `2286:31570`（宽 654px） |

---

## 3. 组件规范

### 3.1 只读信息行

| 属性 | 值 |
|------|------|
| 标签 | 「信息名称」，170px |
| 内容 | 14px Regular `#1C1F23`，与标签间距 8px |
| 行高 | 32px |

### 3.2 数字输入框 `InputNumber`

| 属性 | 值 |
|------|------|
| 场景 | 原价 / 现价 双行组合 |
| 子标签宽 | **56px**（「原价」「现价」） |
| 输入宽 | **200px** |
| 后缀 | **24px** 右对齐（「元」） |
| 行间距 | **8px** |
| 按钮区 | 宽 **14px**，与输入间距 **4px**；上下 chevron 各 **15px** |
| 错误态 | 图标 14×14 + 文案 12px `#F64F4F`，距末行 **8px** |

### 3.3 选择框 `Select`

| 属性 | 值 |
|------|------|
| 宽度 | **100%**（填满 `.form-control-col`） |
| 高度 | **32px** |
| 占位 | 「请选择」 |
| 箭头区 | **32px** |
| 前缀 | 无（普通选择）/ `IconSearch`（搜索选择，见 [assets/README.md](../assets/README.md) §2.2） |

### 3.4 输入框 `Input`

| 属性 | 值 |
|------|------|
| 宽度 | 100% |
| 高度 | **32px** |
| 内边距 | 左右 **12px** |
| 占位 | 「请输入内容」 |
| 字数 | 右下 `0/30`，12px Inter + PingFang |

### 3.5 文本输入框 `TextArea`

| 属性 | 值 |
|------|------|
| 宽度 | 100% |
| 内容区高 | **90px**（总高约 116px 含 counter） |
| 内边距 | **12px × 5px** |
| 字数 | `0/100`，counter 行高 **24px** |

### 3.6 时间 / 日期

| 组件 | 宽度 | 占位 | 后缀图标 |
|------|------|------|----------|
| `TimePicker` | 100% | 请选择时间 | `icon-clock.svg` **16×16**（`1732:122685`；Semi `viewBox 0 0 24 24` + 8.33% inset） |
| `DatePicker`（单日） | 100% | 请选择日期 | `icon-calendar.svg` **16×16**（`1732:122703` · `2283:31341`；同上） |
| `DatePicker`（范围） | 100% | 开始日期 ~ 结束日期 | suffix 区 **32px**；`icon-calendar.svg` **16×16** 居中（`1732:122721`） |

### 3.7 图片上传 `Upload`（picture-card）

画板仅保留 **两种比例** 示例（`2286:31776` · `2286:31792`）：

| 比例 | 卡片尺寸 | 说明文案示例 | 节点 |
|------|----------|--------------|------|
| **1:1** | **96×96** | 示例图片描述文本，比例：1:1 | `2286:31776` |
| **16:9** | **171×96**（画板 `170.67×96`） | 示例图片描述文本，比例：16:9 | `2286:31792` |

**共用样式**

| 属性 | 值 |
|------|------|
| 边框 | 2px dashed `rgba(28,31,35,0.08)` |
| 背景 | `rgba(46,50,56,0.05)` |
| 圆角 | 3px |
| 加号图标 | `icon-plus.svg` **16×16** 居中（Upload / Light Button） |
| 说明 | 卡片下 **8px**，12px 60% 字（Upload 组件 `gap: 8px`） |

### 3.8 添加按钮（`2288:33517`）

| 属性 | 值 |
|------|------|
| 节点 | `2288:33517`（Button） |
| 类型 | Semi **Light Button** |
| 尺寸 | 高 **32px**；**宽随文案**（`width: auto` + `padding: 6px 12px`，非固定宽） |
| 背景 | `color/button_light-bg-default` → `rgba(46,50,56,0.05)` |
| 边框 | 无（`border: transparent`） |
| 圆角 | **3px**（`radius/button`） |
| 内容区 | 图标 16 + gap 8 + 文案；`white-space: nowrap` |
| 图标 | `IconPlus` **16×16**，与文案间距 **8px** |
| 文案 | 「添加内容」14px **Semibold** / 20px · `usage/text/--semi-color-text-1`（80% `#1C1F23`） |
| 辅助说明 | 按钮下 **8px**；说明文案宽随 `.form-control-col`（**不**与按钮等宽） |

### 3.9 单选框 `RadioGroup`

| 属性 | 值 |
|------|------|
| 布局 | 水平 |
| 选项间距 | **16px** |
| 圆点 | **16×16**；选中填充 `#1C5CFB` |
| 文案 | 14px；选中 80% 色，未选中 `#1C1F23` |
| 选项示例 | 选项一（选中）、选项二 ×3 |

### 3.10 复选框 `CheckboxGroup`

| 属性 | 值 |
|------|------|
| 布局 | 水平 |
| 选项间距 | **16px** |
| 框体 | **16×16**，圆角 3px |
| 选中色 | `#0064FA` + 白色勾 |
| 未选中 | 1px inset `rgba(28,31,35,0.35)` |
| 文案 | 14px `#1C1F23`，与框间距 **8px** |

### 3.11 面包屑 `Breadcrumb`

| 属性 | 值 |
|------|------|
| 字号 | 14px Regular |
| 项间距 | `margin-right: 4px` |
| 分隔符 | `/`，颜色与默认项一致 |
| 默认项 | 60% 字色，可点击 |
| 当前项 | `#1C1F23` |

---

## 4. 表单项清单（画板顺序）

| # | 标签 | 控件 | 必填 | 辅助说明 | 分层所属区 |
|---|------|------|------|----------|------------|
| 1 | 信息名称 | 只读文案 | — | — | 填写一 |
| 2 | 数字输入框 | InputNumber ×2（原价/现价） | * + help | 错误描述文本（错误态） | 填写一 |
| 3 | 选择框 | Select | * | 选择框描述文本 | 填写一 |
| 4 | 搜索框 | Select + search 前缀 | * | 搜索框描述文本 | 填写一 |
| 5 | 输入框 | Input + counter 0/30 | * | 输入框描述文本 | 填写一 |
| 6 | 文本输入框 | TextArea + counter 0/100 | * | 文本输入框描述文本 | 填写一 |
| 7 | 时间筛选 | TimePicker | * | 时间筛选描述文本 | 填写二 |
| 8 | 日期筛选 | DatePicker | * | 日期筛选描述文本 | 填写二 |
| 9 | 日期范围筛选 | DatePicker range | * | 日期筛选描述文本 | 填写二 |
| 10 | 示例图片 | Upload **1:1** 96×96 | * | 比例：1:1 | 填写二 |
| 11 | 示例图片 | Upload **16:9** 171×96 | * | 比例：16:9 | 填写二 |
| 12 | 添加按钮 | Light Button | * | 添加描述文本 | 填写二 |
| 13 | 单选框 | RadioGroup ×4 | * | 选项描述文本 | 填写二 |
| 14 | 复选框 | Checkbox ×4 | * | 选项描述文本 | 填写二 |

**分层 · 文案槽位**：每区「信息标题」可独立配置；其余槽位见 §0.2。

---

## 5. HTML / Semi 实现要点

### 5.1 推荐组件映射

图标资产见 [assets/README.md](../assets/README.md) §3.3 · §2.2 · **§2.3（SVG 编码与内联引用）**。  
HTML 金样 [InfoInput.html](../preview/InfoInput.html) 中元信息、help、suffix、upload 等均为**内联 SVG**（不依赖 `<img src="../assets/icons/…">` 二次加载）；React 仍对照 `assets/icons/` 文件名与 Semi 组件。

| 画板组件 | Semi UI |
|----------|---------|
| SelectTrigger | `Select` |
| Input / TextArea | `Input` / `TextArea` + `maxCount` |
| InputNumber | `InputNumber` |
| TimePickerTrigger | `TimePicker` |
| DatePickerTrigger | `DatePicker` / `DatePicker type="dateRange"` |
| Upload | `Upload` `listType="picture"` 或自定义 picture-card |
| Radio / Checkbox | `RadioGroup` / `CheckboxGroup` |
| 添加按钮 | `Button theme="light"` `icon={<IconPlus />}` |
| 预览分段 | `RadioGroup type="button"` `size="small"` |
| 页脚 | 自定义 `form-footer` 或 `SideSheet` footer 样式 |

### 5.2 表单布局代码结构（示意）

**单一信息模块**

```html
<div class="form-row">
  <section class="card form-card">
    <header class="form-header">…</header>
    <hr class="form-divider" />
    <div class="form-body">
      <div class="form-cell" data-field="infoName">…</div>
    </div>
  </section>
  <aside class="card preview-card">…</aside>
</div>
<footer class="form-footer">…</footer>
```

**分层信息模块**

```html
<section class="card form-card">
  <header class="form-header">…</header>
  <hr class="form-divider" />
  <div class="form-sections">
    <section class="form-section" data-section="basic">
      <h3 class="section-title">信息标题</h3>
      <div class="form-body">…</div>
    </section>
    <section class="form-section" data-section="advanced">
      <h3 class="section-title">信息标题</h3>
      <div class="form-body">…</div>
    </section>
  </div>
</section>
```

> **勿**为每个分区单独包白卡；**勿**省略头部分割线。

### 5.3 壳层复用

- 顶栏 / 侧栏：从 [preview/LayoutSpec.html](../preview/LayoutSpec.html) 复制 DOM；样式用 `assets/layout-shell.css` + [LayoutSpec.md](./LayoutSpec.md) §八
- 内容区：`max-width: 1072px`；上内边距 **32px** 由 `layout-shell.css` 提供；固定底栏页可追加 `padding-bottom`（如 96px）
- 页脚 `left` 偏移等于侧栏宽 **272px**

---

## 6. 表单项配置（`FORM_FIELD_CONFIG`）

**适用**：[InfoInput.html](../preview/InfoInput.html)（`applyFormFieldConfig`）

按业务需求控制 **14 项表单项** 的显隐、文案、选项与预览区；默认与画板 `2286:31417`（分层）一致。单一结构见 `variant: "single"`。分层结构见 §6.5 `sections`。

```javascript
// 示例：仅保留输入类 + 单选，隐藏上传与预览
FORM_FIELD_CONFIG.fields.uploadSquare.show = false;
FORM_FIELD_CONFIG.fields.uploadWide.show = false;
FORM_FIELD_CONFIG.fields.checkbox.show = false;
FORM_FIELD_CONFIG.preview.show = false;
FORM_FIELD_CONFIG.fields.radio.options = [
  { label: "类型 A", value: "a", checked: true },
  { label: "类型 B", value: "b" }
];
applyFormFieldConfig(FORM_FIELD_CONFIG);
```

### 6.1 配置结构

| 区块 | 键 | 说明 |
|------|-----|------|
| 页面标题区 | `page.title` · `page.description` | 表单白卡内 18px 标题与描述 |
| 元信息 | `meta.showAccess` · `meta.showGuide` | 当前接入 / 能力接入指引显隐 |
| 元信息文案 | `meta.accessLabel` · `meta.capabilityName` · `meta.guideText` · `meta.guideHref` | |
| 预览区 | `preview.show` · `preview.title` · `preview.tabs[]` · `preview.caption` | 右栏白卡；`tabs` 驱动分段控件 |
| 页脚 | `footer.show` · `footer.cancelText` · `footer.submitText` | 固定底栏 |
| 变体 | `variant` · `sections[]` | `"single"` / `"layered"`；分层字段归属见 §6.5 |
| 表单项 | `fields.<key>` | 见 §6.2 |

### 6.2 字段 key 一览

| key | 控件 | 主要可配项 |
|-----|------|-----------|
| `infoName` | 只读 | `show` · `label` · `value` |
| `numberInput` | InputNumber×2 | `show` · `label` · `required` · `rows[]` · `showError` · `errorText` |
| `select` | Select | `show` · `label` · `required` · `placeholder` · `hint` |
| `searchSelect` | Select+search | 同 `select` |
| `textInput` | Input | `show` · `label` · `required` · `placeholder` · `maxLength` · `hint` |
| `textarea` | TextArea | 同 `textInput` |
| `timePicker` | TimePicker | `show` · `label` · `required` · `placeholder` · `hint` |
| `datePicker` | DatePicker | 同 `timePicker` |
| `dateRange` | DateRange | `startPlaceholder` · `endPlaceholder` · `hint` |
| `uploadSquare` | Upload **1:1** 96×96 | `show` · `label` · `required` · `sizeClass: "size-1x1"` · `hint` |
| `uploadWide` | Upload **16:9** 171×96 | `show` · `label` · `required` · `sizeClass: "size-16x9"` · `hint` |
| `addButton` | Light Button | `show` · `label` · `required` · `buttonText`（宽随文案）· `hint` |
| `radio` | RadioGroup | `show` · `label` · `required` · `hint` · `name` · `options[]` |
| `checkbox` | CheckboxGroup | `show` · `label` · `required` · `hint` · `options[]` |

### 6.3 选项数组格式

**单选 `fields.radio.options`**

```javascript
{ label: "选项一", value: "1", checked: true }
```

**多选 `fields.checkbox.options`**

```javascript
{ label: "选择框一", value: "1", checked: true }
```

**数字输入 `fields.numberInput.rows`**

```javascript
{ label: "原价", suffix: "元", value: "0" }
```

### 6.4 显隐规则

| 条件 | 行为 |
|------|------|
| `fields.<key>.show: false` | 对应 `.form-cell[data-field]` 添加 `.is-hidden` |
| `preview.show: false` | 隐藏 `.preview-card`，左表单占满 |
| `meta.showAccess: false` | 隐藏「当前接入」组 |
| `meta.showGuide: false` | 隐藏「能力接入指引」链接 |
| `footer.show: false` | 隐藏固定页脚 |

面包屑与全局壳层 **不受** `FORM_FIELD_CONFIG` 控制。

### 6.5 分层结构配置（`sections`，`2286:31417`）

在 `FORM_FIELD_CONFIG` 上扩展 `sections`，控制分区标题、字段归属与整区显隐：

```javascript
FORM_FIELD_CONFIG.sections = [
  {
    key: "basic",
    show: true,
    title: "信息标题",
    fields: ["infoName", "numberInput", "select", "searchSelect", "textInput", "textarea"]
  },
  {
    key: "advanced",
    show: true,
    title: "信息标题",
    fields: ["timePicker", "datePicker", "dateRange", "uploadSquare", "uploadWide",
             "addButton", "radio", "checkbox"]
  }
];
applyFormFieldConfig(FORM_FIELD_CONFIG);
```

| 规则 | 行为 |
|------|------|
| `section.show: false` | 隐藏整区（含信息标题） |
| `section.fields` | 控制字段归属与顺序；字段 `show: false` 在区内跳过 |
| 区内无可见字段 | 整区视同隐藏 |
| `page` / `meta` / `preview` / `footer` | 与单一模块相同 |

---

## 7. 交付检查清单

**共用**

- [ ] 左/右 **白底卡片**（`#FFF`、圆角 12px）浮于灰底内容区
- [ ] 面包屑多级路径与当前项高亮（**在**白卡 **外**）
- [ ] 表单双栏 24px 间距，预览宽 330px 不压缩
- [ ] 标签列 171px、控件区 flex:1、行 gap 24px
- [ ] 必填 `*`、辅助说明 12px、错误态红色
- [ ] 纯输入控件（Input / Select / TextArea / 时间日期）**width: 100%** 填满控件列
- [ ] 添加按钮 **宽随文案**（Light Button · `2288:33517`）
- [ ] 两类上传比例（**1:1** 96×96 · **16:9** 171×96）与虚线框样式
- [ ] 预览区分段切换 + Skeleton 298×530
- [ ] 底栏固定 80px，取消/提交右对齐
- [ ] 主内容区底部留白 ≥48px
- [ ] 卡片区域**无**单独滚动（`max-height` / `overflow: auto`），内容完全展示

**分层信息模块附加**

- [ ] 表单头 + 分割线 + **≥1** 个带「信息标题」的分区
- [ ] 信息标题 14px Semibold；`.form-section` gap **24px**（标题 → 首字段）
- [ ] `.form-sections` 分区间 **24px**；空分区不展示悬空标题

**info-entry 模版自检（合并）**

- [ ] 已套平台壳（顶栏 60px + 侧栏 272px）；面包屑在白卡外
- [ ] 无实时预览需求时未放置预览列
- [ ] 预览 Tab 1～3；**仅 1 个时无右上角 Tab**；预览骨架比例 **9:16**（298×530）
- [ ] 页面标题下 Divider：左右各 **32px**，宽度为 formScroll 内容区宽（非贴白卡边）
- [ ] 无预览：白卡全宽，纯输入控件 **width: 100%**，最大宽 `--form-column-max-width`
- [ ] 信息块 = 1 时无块级「信息标题」；≥2 时每块有标题
- [ ] Form：`labelPosition=left`，`labelWidth=171`，标签字重 **400**
- [ ] 表单项使用 Semi 组件（非纯矩形）；错误文案仅在校验失败时出现

---

## 8. 模版契约（`template.yaml`）

Agent 生成 Figma 设计稿、React 或 HTML 预览前 **先读本节**。

### 8.1 区域（regions）

| id | 必填 | 说明 |
|----|------|------|
| `breadcrumb` | 否 | 白卡外，Semi `Breadcrumb` |
| `pageHeader` | 是 | 白卡内：标题（18px Semibold）+ 描述 + 可选 `extra`（元信息） |
| `headerDivider` | 是 | Semi `Divider`；左右 **32px** 内边距，宽 = formScroll 内容区 |
| `formCard` | 是 | 白卡内容完全展示（无卡内滚动）；子级 `infoSections[]` |
| `preview` | 否 | `realtimePreview=true` 时；宽 **330px**，Tab 最多 **3** |
| `footer` | 是 | 固定底栏；画板 **80px**（Figma）/ React 金样 **64px** |

### 8.2 预览规则

| 键 | 值 |
|----|-----|
| `preview.enabled` | 仅「输入可实时预览」为 true |
| `preview.maxTabOptions` | **3** |
| `preview.showTabsWhen` | `options.length > 1` |
| `preview.cardWidth` | **330** |
| `preview.imageAspectRatio` | **9/16** |
| `preview.gapToFooter` | **24px** |

### 8.3 表单规则

| 键 | 值 |
|----|-----|
| `form.labelWidth` | **171** |
| `form.labelAlign` | left |
| `form.labelFontWeight` | **400** |
| `form.infoSection.showBlockTitleWhen` | `sectionCount > 1` |
| `form.infoSection.blockTitleText` | 「信息标题」 |
| `form.infoSection.sectionGap` | **24px** |
| `form.noPreview.contentMaxWidth` | `calc(100% - 330px - 24px)` |

### 8.4 字段目录（`formFieldCatalog`）

| id | Semi |
|----|------|
| `readonlyText` | `Form.Slot` |
| `inputNumberGroup` | `Form.Slot` + `Form.InputNumber` |
| `select` / `input` / `textarea` | `Form.Select` / `Form.Input` / `Form.TextArea` |
| `timePicker` / `datePicker` / `dateRange` | `Form.TimePicker` / `Form.DatePicker` |
| `uploadPicture` | `Form.Upload` `listType=picture` |
| `radioGroup` / `checkboxGroup` | `Form.RadioGroup` / `Form.CheckboxGroup` |

### 8.5 Figma 参考节点

| 键 | 节点 |
|----|------|
| `fileKey` | `8ca5RrQYK8JRq7ZNJh3xsA` |
| `pageNode`（单一） | `1732:122528` |
| `moduleNode` | `2163:22551` |
| `previewNode` | `2163:23079` |
| 分层页 | `2286:31417` |

---

## 9. Semi ↔ Figma 映射（`semi-figma-map.yaml`）

生成 Figma 时：`search_design_system` 优先搜 **figmaSearch** 列，**禁止**手绘矩形替代 Form/Upload。

| Semi | figmaSearch 关键字 |
|------|-------------------|
| `Breadcrumb` | Breadcrumb, 面包屑 |
| `Divider` | Divider, 分割线 |
| `Typography.Title` heading=5 | Title, 页面标题 |
| `Form` | Form, 表单 |
| `Form.Input` / `Form.Select` / `Form.TextArea` | Input, Select, TextArea |
| `Form.InputNumber` | InputNumber, 数字输入框 |
| `Form.TimePicker` / `Form.DatePicker` | TimePicker, DatePicker |
| `Form.Upload` | Upload, 上传, picture |
| `Form.RadioGroup` / `Form.CheckboxGroup` | RadioGroup, CheckboxGroup |
| `RadioGroup` type=button | Radio, 选项（预览 Tab） |
| `Button` | Button, 按钮 |

**CSS Token**

| 变量 | 值 | 用途 |
|------|-----|------|
| `--spacing-extra-loose` | 32px | 内容区左右 padding、Divider inset |
| `--spacing-loose` | 24px | 分区间距、主行 gap |
| `--preview-card-width` | 330px | 预览卡宽 |
| `--form-column-max-width` | `calc(100% - var(--preview-card-width) - var(--spacing-loose))` | 无预览时表单列宽 |
| `--footer-bar-height` | 64px | React 金样底栏 |
| `--nav-sider-width` | 272px | 侧栏宽 |

---

## 10. 生成工作流

**壳层（先于 §10.1–§10.3）**：LayoutSpec.md §八 → 复制 `LayoutSpec.html` 壳层 → `layout-shell.css`；**图标**：`assets/README.md` §3.3 → `assets/icons/*.svg`；业务仅填入 `.content-area > .main`。

### 10.1 Figma 设计稿

1. **解析需求** → 确定 `preview.enabled`、`infoSection.count`、字段子集
2. **加载** `figma-use` / `figma-generate-design`（MCP 写入前必读）
3. **`search_design_system`**：按 §9 拉 Semi 组件
4. **拼装顺序**：面包屑 → 白卡 → 页面标题区 → **Divider** → 信息块 cell → 预览卡（若有）→ 底栏
5. **对照** [InfoInput.html](../preview/InfoInput.html) 或 `SingleInfoModulePage` 金样
6. **自检** §7 清单

### 10.2 React 代码

```tsx
import { PageWithPreviewLayout } from '@/layouts/PageWithPreviewLayout';

<PageWithPreviewLayout
  breadcrumb={<Breadcrumb routes={...} />}
  header={{ title: '...', description: '...', extra: <>...</> }}
  preview={
    needPreview
      ? { options: tabs, children: <Preview />, caption: '...' }
      : undefined
  }
  footer={<><Button>取消</Button><Button type="primary">提交</Button></>}
>
  {/* 各信息块 Form；sectionCount > 1 时加 SectionTitle */}
</PageWithPreviewLayout>
```

| Prop | 规则 |
|------|------|
| `preview` | 不传 = 无预览；表单白卡全宽 |
| `preview.options` | 最多 3；`length <= 1` 不渲染 `RadioGroup` |
| `children` | 仅表单/信息块，不含页面标题与 footer |
| `footer` | 底栏按钮区 |

**金样路径**：`src/layouts/PageWithPreviewLayout/` · `src/pages/SingleInfoModulePage/`

### 10.3 HTML 预览

- 文件：[InfoInput.html](../preview/InfoInput.html)
- 配置：`FORM_FIELD_CONFIG` + `applyFormFieldConfig()`
- 分层：设置 `sections` 数组（§6.5）；单一模块省略 `sections`

### 10.4 禁止项

- 卡片区域内单独设置 `overflow: auto` / `max-height` 截断表单（应完全展示）
- 无预览时把 `Form.Input` 拉满大屏（须受 `--form-column-max-width` 约束）
- Divider 贴白卡左右边缘（须保留 **32px** padding 宽）
- 预览仅 1 项仍画「选项一/二」Tab
- 超过 **3** 个预览 Tab
- 为每个分区单独包白卡（分层须在**同一**左白卡内）

---

*节点索引*  
- **单一**：`1732:122528`（页）· `1732:122535`（表单行）· `1732:122536`（左卡）· `2173:26619`（预览）· `1816:118365`（页脚）  
- **分层**：[2286:31417](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit?node-id=2286-31417)（页）· `2286:31547`（左卡）· `2286:31571` / `2286:31683`（填写分区）· `2286:33426`（预览）· `2286:31956`（页脚）
