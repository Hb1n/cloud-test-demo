---
name: 互动游戏B端模版
description: >-
  抖音开放平台 / 互动游戏 UI Kit B 端页面模版库。生成任意页面前必读 LayoutSpec 壳层金样
  （LayoutSpec.md §八 + LayoutSpec.html + layout-shell.css + app-avatar.png），业务图标见
  assets/README.md §3，再读对应 references 与 preview；含能力介绍、信息查询、数据看板、信息录入、弹窗等模版。
---

# 互动游戏 B 端模版 Skill

## 何时使用

- 用户要求 **生成 B 端页面**、**按 UI Kit 模版实现**、**从 Figma 还原**
- 涉及 **能力介绍**、**能力列表**、**表格**、**筛选**、**弹窗**、**开放平台控制台** 布局
- 需要统一 Semi Design token、Tabs、面包屑、指标卡、StatesTag 等样式

---

## 使用方式（补充说明）

本 Skill 是 **入口索引**，不替代各 `references/*.md` 的完整规范。按下列方式使用可保证生成结果与 UI Kit 一致。

### 快速路径（推荐）

```
1. 确认页面类型（介绍 / 列表 / 录入 / 看板 / 弹窗）
2. 查「生成必读矩阵」对应行 → 依次打开列出的文档与 HTML 金样
3. 按「生成工作流与自检」①→⑤ 执行（壳层先于业务）
4. 需要变体或字段显隐时，再读下方对应配置节（PAGE_* / LIST_* / FORM_* 等）
5. 细节尺寸、组件、Figma 节点 → 以业务 references/*.md 为准
```

### 按交付形态

| 形态 | 做法 |
|------|------|
| **HTML 静态页** | 复制 `LayoutSpec.html` 壳层 → `link` `layout-shell.css` → 业务区对照 `preview/<页面>.html` 结构与 class → 图标见 [assets/README.md](./assets/README.md) §2.3（静态 `<img>` 或 **JS 动态用内联 SVG**） |
| **React / Semi** | 壳层按 LayoutSpec §8.3（fixed 顶栏 + fixed 侧栏，勿 `Layout.Sider` 内滚）；业务用 `@douyinfe/semi-ui`；图标用 `@douyinfe/semi-icons` 或 `assets/icons/*.svg`；录入页优先 `PageWithPreviewLayout` |
| **Figma 还原** | 先 `LayoutSpec.md` + 业务 reference 的 Figma 节点；录入页另读 InfoInput.md §8–§9 模版契约与 Semi↔Figma 映射 |

### 文档分工

| 文档 | 职责 |
|------|------|
| **SKILL.md**（本文件） | 何时用、必读矩阵、工作流、Token 摘要、配置 API 速查 |
| **references/LayoutSpec.md** | 壳层尺寸、结构、禁止项、§八金样约束 |
| **references/<业务>.md** | 该页面完整布局、组件、间距、Figma 节点、生成清单 |
| **preview/*.html** | 可运行的 HTML 金样（壳层以 `LayoutSpec.html` 为唯一来源） |
| **assets/README.md** | 图标文件索引、Semi 映射、按页面 §3 引用、资产自检 |

### 常见错误（禁止）

- 从 **业务** preview（如 InfoQuery.html）复制 Header / 侧栏，而非 `LayoutSpec.html`
- 将 `.content-area` padding 写成四边 `24px`，或从业务 preview 反推 padding
- 只写 `semi-icons-*` / `IconX` 名称而不引入 SVG 或 Semi 组件
- 使用无 sprite 定义的 `<use href="#icon-…">`（图标会空白）
- **`assets/icons/*.svg` 以 ISO-8859 / Latin-1 保存**（注释含 `·` `×` → 浏览器 `Encoding error`，图标空白；见 assets/README §2.3）
- JS `innerHTML` 注入 `<img src="../assets/icons/…">` 而不优先内联 SVG（动态区易空白；见 assets/README §2.3）
- 列表「任务状态」用 Large `Tag` 代替 `StatesTag`
- 信息录入对 `.form-card` 设 `max-height` / `overflow: auto` 导致卡片内滚动

### 示例：生成「单个能力列表页」

1. 矩阵行「信息查询」→ 读 LayoutSpec §八、`LayoutSpec.html`、`layout-shell.css`、`app-avatar.png`
2. `assets/README.md` §3.2 → 引入元信息与表格图标
3. `InfoQuery.md` + `InfoQuery.html` → `resolveListVariant` 得 `single`，`tabLevels: 1`
4. 仅在 `.content-area > .main` 填入 intro + Line Tabs + `list-panel`
5. 自检：padding 32/24/24 · 元信息图标可见 · 表格行高 64 · StatesTag

---

## 生成必读矩阵

**所有页面**均先完成壳层，再读业务文档。下表为各类型完整必读清单：

| 页面类型 | 壳层（强制） | 图形资产 | 业务规范 | HTML 金样 |
|----------|--------------|----------|----------|-----------|
| **任意页面** | [LayoutSpec.md](./references/LayoutSpec.md) §八 · [LayoutSpec.html](./preview/LayoutSpec.html) · `layout-shell.css` · `app-avatar.png` | [assets/README.md](./assets/README.md) §0 | — | [LayoutSpec.html](./preview/LayoutSpec.html) |
| 能力介绍 | 同上 | §3.1 | [CapabilityIntro.md](./references/CapabilityIntro.md) | [CapabilityIntro.html](./preview/CapabilityIntro.html) |
| 信息查询 / 列表 | 同上 | §3.2 | [InfoQuery.md](./references/InfoQuery.md) | [InfoQuery.html](./preview/InfoQuery.html) |
| 信息录入 | 同上 | §3.3 · §2.2 | [InfoInput.md](./references/InfoInput.md) | [InfoInput.html](./preview/InfoInput.html) |
| 数据看板 | 同上 | §3.4 | [Dashboard.md](./references/Dashboard.md) | [Dashboard.html](./preview/Dashboard.html) |
| 弹窗（含壳层展示页） | 同上 | 仅壳层 | [Modal.md](./references/Modal.md) | [Modal.html](./preview/Modal.html) |

**共用规范板**（列表 / 弹窗表格）：表格筛选 `2185:84044` · 标签 `2186:90908`（详见 InfoQuery.md）

---

## 执行步骤

1. **壳层** → 按上表「壳层」列；**仅从** `LayoutSpec.html` 复制 Header + 侧栏 DOM（含内联 SVG）；引入 `layout-shell.css` + `app-avatar.png`
2. **图形资产** → [assets/README.md](./assets/README.md) §0 · §3；HTML 用 `assets/icons/*.svg` 的 `<img>`，禁止只写 `semi-icons-*` 或无 sprite 的 `<use href="#icon-…">`
3. **业务** → 按页面类型读规范 + HTML 金样；仅在 `.content-area > .main` 内拼装业务
4. **组件库** → `@douyinfe/semi-ui`；主色 `#1C5CFB`，选中底 `#EAF5FF` / `#EDF3FF`
5. **变体配置** → 介绍页 `PAGE_TEMPLATE_CONFIG` · 列表 `resolveListVariant` · 录入 `FORM_FIELD_CONFIG` · 弹窗 `resolveModalSize`（见下方各节）

---

## 模版索引

| 模版 | 文档 | Figma 节点 | 预览 / 资产 |
|------|------|------------|-------------|
| 全局壳层 | [LayoutSpec.md](./references/LayoutSpec.md) | `13:9326` | [LayoutSpec.html](./preview/LayoutSpec.html) |
| 图形资产 | [assets/README.md](./assets/README.md) | — | `assets/layout-shell.css` · `assets/app-avatar.png` · `assets/icons/*.svg` |
| 能力介绍 | [CapabilityIntro.md](./references/CapabilityIntro.md) | `1878:102218` · `1886:87371` | [CapabilityIntro.html](./preview/CapabilityIntro.html) |
| 信息查询 | [InfoQuery.md](./references/InfoQuery.md) | `1653:120277` · `1634:68043` · `1665:217264` | [InfoQuery.html](./preview/InfoQuery.html) |
| 数据看板 | [Dashboard.md](./references/Dashboard.md) | `2212:109109` | [Dashboard.html](./preview/Dashboard.html) |
| 信息录入 | [InfoInput.md](./references/InfoInput.md) | `1732:122528` · `2286:31417` | [InfoInput.html](./preview/InfoInput.html) |
| 弹窗 | [Modal.md](./references/Modal.md) | `916:50798` | [Modal.html](./preview/Modal.html) |

---

## 关键 Token 与尺寸

| 项 | 值 |
|----|-----|
| 页面最小宽度 | **960px** |
| Header | **60px** 高 · `fixed` · `z-index: 100` · 左内边距 **32px** |
| 侧栏 | **272px** 宽 · `fixed` · `top: 60px` · 内边距 **24px** · **禁止**内部滚动 |
| 主色 / 选中底 | `#1C5CFB` · `#EAF5FF` / `#EDF3FF` |
| 主文 / 弱化 | `#1C1F23` · `rgba(28,31,35,0.6)` |
| 页面底 / 卡片 | `#F9F9F9` · `#FFFFFF` |
| 控件灰底 / 危险色 | `rgba(46,50,56,0.05)` · `#F93920` |
| 内容区内边距 | **`32px 24px 24px`**（`layout-shell.css`；勿改为四边 24px；勿从业务 preview 反推） |
| 业务内容宽 | `.main` **max-width: 1072px** 居中（卡片内边距 24px） |
| 圆角 | 卡片 **12px** · 控件 **3px** · 侧栏头像 **6px** |
| 侧栏头像 | `app-avatar.png` 80×80 → 展示 40×40 + `rgba(0,0,0,0.2)` 蒙版 |
| 间距栅格 | 4 / 8 / 12 / 16 / **24** / **32** / **40** px（`--space-1`～`--space-7`，见 LayoutSpec §六） |

---

## 壳层金样（强制）

金样三件套：`LayoutSpec.md`（规范）· `LayoutSpec.html`（DOM）· `layout-shell.css`（样式）。**禁止**用 Semi `Layout.Sider` 内滚替代；**禁止**从业务 preview 复制壳层。

| 规则 | 说明 |
|------|------|
| HTML 结构 | `.layout-body` → `.sidebar-column`（272px 占位）→ `.sidebar` + `.content-area` → `.main` |
| 侧栏 | `position: fixed` · `overflow: visible` · `z-index: 2` |
| 白底 | `.sidebar-column::before` fixed · `z-index: 1` |
| 滚动 | 仅内容区滚动；侧栏与白底固定 |
| padding | 默认 **32/24/24**；仅可追加 `padding-bottom`（如录入页固定底栏 **96px**） |
| 禁止 | 覆盖 `.sidebar` / `.sidebar-column` 的 `position` / `overflow`；侧栏 `overflow-y: auto` |

```html
<div class="layout-body">
  <div class="sidebar-column">
    <aside class="sidebar">…</aside>
  </div>
  <div class="content-area">
    <div class="main"><!-- 仅此处放业务内容 --></div>
  </div>
</div>
```

### HTML 拼装顺序（LayoutSpec §8.2）

1. `link` `layout-shell.css`（`app-avatar.png` 与 CSS 同目录）
2. 复制 `LayoutSpec.html` 的 `<header>` + `.layout-body` 侧栏（可改菜单选中态，不改结构与 class）
3. `.content-area` 内仅填 `.main` 业务内容
4. 除 `padding-bottom` 外勿重写 `.content-area` padding

### React / Semi（LayoutSpec §8.3）

| HTML 金样 | React 要求 |
|-----------|------------|
| `.header` fixed 60px | 顶栏独立 fixed，勿包进可滚动 `Layout` |
| `.sidebar` + `::before` + 占位列 | 勿用 `Layout.Sider` 内滚；fixed + `overflow: visible` |
| `.content-area` | 页面级滚动；padding **32/24/24** |
| `.main` 1072px | 业务内容水平居中 |

| 场景 | 做法 |
|------|------|
| 信息录入 | `PageWithPreviewLayout`（InfoInput.md §10，含壳层与底栏） |
| 其他页面 | fixed 顶栏 + fixed 侧栏 + 内容区滚动 |
| 禁止 | `Layout` + `Sider` 默认可滚动方案 |

---

## 图形资产

**唯一文档**：[assets/README.md](./assets/README.md)（含图标索引、Semi 映射、**SVG 编码 §2.3**、样式变量、自检清单）。

| 页面 | assets/README | 需引入 svg（均在 `assets/icons/`） |
|------|---------------|-------------------------------------|
| 能力介绍 | §3.1 | `icon-mobile-app.svg` · `icon-brief-stroked.svg` · `metric-value-rise.svg` |
| 信息查询 | §3.2 | `icon-mobile-app.svg` · `icon-brief-stroked.svg` · `icon-semi-logo.svg` · `icon-action-more.svg` |
| 信息录入 | §3.3 · §2.2 | `icon-pulse.svg` · `icon-brief-stroked.svg` · `icon-help-circle-16.svg` · `icon-chevron-down.svg` · `icon-search.svg` · `icon-clock.svg` · `icon-calendar.svg` · `icon-plus.svg` |
| 数据看板 | §3.4 | `icon-calendar-clock.svg` · `icon-export.svg` · `icon-help-circle.svg` · `icon-chevron-down.svg` · `icon-search.svg` |
| 弹窗 | — | 仅壳层 |

**图标资产目录**：`Games-design-skill/assets/icons/`（共 15 个 svg，均已入库，见 [assets/README.md](./assets/README.md) §2）。

**不在 `assets/icons/` 内**：壳层导航 SVG（`LayoutSpec.html` 内联）· Modal 插图 · Upload 空态 · 应用示例媒体 · 折线图（见 assets/README §5）。

**HTML 图标路径约定**：

| 引用位置 | 路径 |
|----------|------|
| `preview/*.html` | `../assets/icons/<file>.svg` |
| 生成页与 `assets/` 同级 | `assets/icons/<file>.svg` |
| `references/*.md` 文档链接 | `../assets/icons/<file>.svg` |

**本地预览 `Games-design-skill/preview/*.html`**：

1. 用 Cursor 打开 **`Design skill`** 文件夹（仓库根目录）
2. **方式 A（推荐）**：安装 Live Server 扩展 → 右键 `Games-design-skill/preview/index.html` → Open with Live Server
3. **方式 B**：在仓库根目录终端执行 `python3 -m http.server 5503`，浏览器打开 `http://127.0.0.1:5503/Games-design-skill/preview/index.html`
4. **须起 HTTP 服务**；勿用 `file://` 作为唯一预览手段。预览在 `Games-design-skill/preview/`，资产 `../assets/`
5. **SVG 编码**：全部 `assets/icons/*.svg` 须 UTF-8（注释优先 ASCII）；浏览器打开 svg 若报 `Encoding error` → 读 assets/README §2.3 修复后再预览
6. **动态图标**：表格行 / 指标 help / 表单 suffix 等 JS 注入场景 → 金样用**内联 SVG**（InfoInput · Modal）；勿仅依赖 `<img src="…">`

---

## 页面类型速查

### 能力介绍

| Tab 层级 | 变体 | Figma | 要点 |
|----------|------|-------|------|
| 1 层 | 单个能力 | `1878:102218` | Line Tabs；元信息在 intro |
| ≥ 2 层 | 合集能力 | `1886:87371` | Line + Button；元信息在 info 子能力区 |
| > 2 层 | 沿用合集扩展 | — | 外层 Line、内层 Button 类推 |

主 CTA「开始接入」。应用示例：单个 1～3 项 `gap` 52px、4 项铺满无分页；合集 4 列 `justify-between`，>4 项分页。

### 信息查询

**决策优先级**：① 筛选外置 / 仅功能标题 → `filter-outside`（`1665:217264`）② Tab ≥2 层 → `collection`（`1634:68043`）③ 默认 → `single`（`1653:120277`）

| 变体 | Tab | 筛选 | 标题区 | 列表容器 |
|------|-----|------|--------|----------|
| 单个 | Line | 嵌入 `list-panel` | intro + **元信息** | `list-panel` |
| 合集 | Line + Button | 嵌入 `info` | intro（无元信息） | `info` |
| 外置 | Line + Button | 独立 `filter-panel` | 仅功能标题 | `info` + CardTittle |

列表页用 `LIST_PAGE_CONFIG` / `resolveListVariant`；禁止外置时把筛选写入 `info`；任务状态列用 `StatesTag`。

### 其他页面

| 页面 | 布局要点 |
|------|----------|
| 数据看板 | 功能标题 · Line + Button Tabs · 独立 `filter-panel` + `data-panel`（4×2 指标卡 + 折线图） |
| 信息录入 | 面包屑 · 表单 + 可选预览双栏 · 固定底栏 · 卡片不单独滚动 · `padding-bottom: 96px`；分层时 `.form-section` gap **24px**；纯输入 **width: 100%**；添加按钮 **宽随文案**；上传 **1:1** / **16:9** |
| 弹窗 | 尺寸 448/684/920 · 内容三类 · `PlatformModal`；展示页含完整壳层 |

---

## 介绍页配置

**适用**：[CapabilityIntro.html](./preview/CapabilityIntro.html) · **规范**：[CapabilityIntro.md](./references/CapabilityIntro.md)  
**顺序**：`applyPageTemplate` → `applyPageContentConfig`

### 模版选型（`PAGE_TEMPLATE_CONFIG`）

| 配置项 | 说明 |
|--------|------|
| `tabLevels` | `1` 单个 · `≥2` 合集 |
| `page.title` / `page.desc` | intro 标题与描述 |
| `subCapabilities[]` | ≥2 层 Line Tabs |
| `functionTabs[]` | 功能 Tab |
| `subCapability.title` / `desc` | 合集 info 子能力区 |

```javascript
PAGE_TEMPLATE_CONFIG.tabLevels = 2;
applyPageTemplate(PAGE_TEMPLATE_CONFIG);
```

URL：`CapabilityIntro.html?tabs=1` · `?tabs=2`。详情 §6。

### 内容配置（`PAGE_CONTENT_CONFIG`）

控制 info 三块：**能力价值** / **文字描述** / **应用示例**。

```javascript
PAGE_CONTENT_CONFIG.capabilityValue.show = false;
PAGE_CONTENT_CONFIG.applicationExamples.show = false;
applyPageContentConfig(PAGE_CONTENT_CONFIG);
```

| 模块 | 展示条件 | 隐藏时 |
|------|----------|--------|
| 能力价值 | `show` 且标题/描述/CTA/指标至少一项 | `.info-section.is-hidden` |
| 指标卡 | `showMetrics` 且 `metrics` ≥ 2 | 隐藏指标网格 |
| 文字描述 | `show` 且 `items.length > 0` | 整段隐藏 |
| 应用示例 | `show` 且 `items.length > 0` | 整段隐藏 |
| 三块全隐 | — | `.card.info.is-content-empty` |

intro 卡片不受 `show` 影响。子开关：`showTitle` · `showIntro` · `showMetrics` · `showCta` · `ctaText`。详情 §5。

---

## 信息查询配置（`LIST_PAGE_CONFIG`）

**适用**：[InfoQuery.html](./preview/InfoQuery.html) · **规范**：[InfoQuery.md](./references/InfoQuery.md)  
**顺序**：`resolveListVariant` → `applyListPageConfig`

| 配置项 | 说明 |
|--------|------|
| `variant` | `single` \| `collection` \| `filter-outside` |
| `tabLevels` | `1` 单个 · `≥2` 合集 |
| `filterPlacement` | `inside`（默认）\| `outside` |
| `page.title` / `description` | intro 文案 |
| `page.functionTitle` | 筛选外置功能标题 |
| `page.introMeta` | 单个能力元信息 |
| `tabs.*` | 各变体 Tab 文案 |
| `list.cardTitle` | 外置时 CardTittle |
| `filter.mode` | `simple` \| `standard` \| `multi-row` |

```javascript
function resolveListVariant(cfg) {
  if (cfg.variant) return cfg.variant;
  if (cfg.filterPlacement === "outside") return "filter-outside";
  if ((cfg.tabLevels || 1) >= 2) return "collection";
  return "single";
}
applyListPageConfig({ filterPlacement: "outside" });
```

| 变体 | 默认 Tab | 主操作 |
|------|----------|--------|
| 单个 | Line「能力介绍」 | 「新建能力」 |
| 合集 | Button「能力管理」 | 「新建能力」 |
| 外置 | Button「全部」 | 筛选「查询/重置」 |

详情 §0 · §5 · §7。

### 列表页要点

- **筛选**：`simple`（默认）\| `standard` \| `multi-row`（外置默认）
- **表格**：6 列 · 行高 64px · 内容宽 1024px；操作色 `#1C5CFB` / `#888D92` / `#F93920`
- **标签**：列表 `StatesTag` · 标签列 `Tag Small`（`2186:90908`）

---

## 信息录入配置（`FORM_FIELD_CONFIG`）

**适用**：[InfoInput.html](./preview/InfoInput.html) · **规范**：[InfoInput.md](./references/InfoInput.md)  
**默认金样**：`variant: "layered"` · 画板 `2286:31417`（分层信息模块）

| 配置项 | 说明 |
|--------|------|
| `fields.<key>.show` | **14** 项显隐 |
| `preview.show` | 关预览 → 表单全宽 |
| `preview.tabs` | 1～3 项；仅 1 项时不渲染 Tab |
| `sections[]` | 分层结构；`sectionCount > 1` 显示「信息标题」；`.form-section` / `.form-sections` 间距 **24px** |
| `variant` | `single` \| `layered` |
| `fields.uploadSquare` | 示例图片 **1:1** · `sizeClass: "size-1x1"` · 96×96 |
| `fields.uploadWide` | 示例图片 **16:9** · `sizeClass: "size-16x9"` · 171×96 |
| `page` / `meta` / `footer` | 标题、元信息、底栏 |

禁止 `.form-card` 设 `max-height` / `overflow: auto`。Figma/React 读 §8–§9；React 用 `PageWithPreviewLayout`。详情 §6 · §10。

---

## 弹窗配置（三维度）

**适用**：[Modal.html](./preview/Modal.html) · **规范**：[Modal.md](./references/Modal.md)  
组件 **`PlatformModal`** · 决策：内容类型 → `resolveModalSize` → 确认 preset

| 维度 | 规则 |
|------|------|
| 尺寸 | small/medium/large = **448/684/920**；优先最小档 |
| 内容 | 2.1 插图 · 2.2 展示（含表格）· 2.3 输入 |
| 确认 | 标题+关闭必填（插图样式一 `closableOnly` 例外）；`confirm` 蓝主按钮 · `danger` 红 `#F93920` |

| 子类 | 要点 |
|------|------|
| 2.1 插图 | `1150:15525` 顶图 200px · `1150:15778` 居中 16:9 |
| 2.2 表格 | medium 默认 · 六列均分 · 筛选 216×32 · `StatesTag` / `Tag Small` |
| 2.3 输入 | 左标签 145px · 无上传 · 宽度随弹窗自适应 |

详情 §0 · §3 · §4 · §7。

---

## 数据看板配置（`METRIC_CONFIG`）

**适用**：[Dashboard.html](./preview/Dashboard.html) · **规范**：[Dashboard.md](./references/Dashboard.md)

`columns`（1～4）+ `items` 控制指标卡；默认 8 项 / 4 列。详情 §3.7.5。

---

## 生成工作流与自检

```
① 壳层
   LayoutSpec.md §八 → LayoutSpec.html（复制 Header + 侧栏，含内联 SVG）
   → layout-shell.css + app-avatar.png
         ↓
② 图形资产
   assets/README.md §0 + §2.3 + §3（按页面类型）→ assets/icons/*.svg（UTF-8）
         ↓
③ 业务金样（见「生成必读矩阵」）
         ↓
④ 仅在 .content-area > .main 拼装业务 + 应用配置 API
         ↓
⑤ 自检
   □ 壳层非业务 preview 复制；侧栏无内滚；padding 32/24/24
   □ app-avatar.png 可加载；业务图标可见；无空 sprite
   □ svg 文件 UTF-8（`file …/*.svg` 无 ISO-8859）；直接打开 svg 无 Encoding error
   □ JS 动态图标已用内联 SVG 或编码正确的 `<img>`
   □ 列表 StatesTag · 录入底栏留白 · 录入分层 `.form-section` gap 24px · 上传 1:1/16:9 · 弹窗宽度三档之一
```

---

## 交付物约定

| 类型 | 路径 |
|------|------|
| Skill 入口 | `Games-design-skill/SKILL.md` |
| 页面规范 | `Games-design-skill/references/*.md` |
| HTML 金样 | `preview/*.html` |
| 图形资产 | `Games-design-skill/assets/README.md` · `assets/layout-shell.css` · `assets/app-avatar.png` · `assets/icons/*.svg` |

*Figma 源文件：[互动游戏_平台产品-UI-Kit](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA)*
