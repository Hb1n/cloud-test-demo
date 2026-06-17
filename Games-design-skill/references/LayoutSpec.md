# 抖音开放平台 — 布局规范文档

> 来源：Figma 画板 `fye7eRpGBb6Od0BBXLbMV2` · Node `13:9326`  
> HTML 实现：`../preview/LayoutSpec.html` · 壳层 CSS：`assets/layout-shell.css`  
> 更新日期：2026-06-11

---

## 一、页面整体结构

```
┌──────────────────────────── 100vw (min-width: 960px) ────────────────────────────┐
│  Header  height: 60px  position: fixed  top:0 left:0 right:0  z-index:100        │
├──────────────┬───────────────────────────────────────────────────────────────────┤
│ sidebar-     │  Content Area（随页面滚动）                                        │
│ column 272px │  flex:1  min-width:0  background:#F9F9F9                           │
│ 文档流占位   │  padding: 32px 24px 24px（见 layout-shell.css）                    │
│ + 固定白底   │                                                                    │
│ + 固定侧栏   │                                                                    │
└──────────────┴───────────────────────────────────────────────────────────────────┘
```

### HTML 结构（所有 preview 模版统一）

```html
<div class="layout-body">
  <div class="sidebar-column">
    <aside class="sidebar">…</aside>
  </div>
  <div class="content-area">…</div>
</div>
```

样式见 `assets/layout-shell.css`。

### 关键尺寸

| 区域 | 属性 | 值 |
|---|---|---|
| 页面最小宽度 | `min-width` | `960px` |
| Header | `height` | `60px` |
| Header | `position` | `fixed`，`left:0 right:0` 全宽自适应 |
| Header | `z-index` | `100` |
| Sidebar 占位 | `.sidebar-column` `width` | `272px`（文档流占位，`align-self: stretch`） |
| 白色背景 | `.sidebar-column::before` | `position: fixed`，`top: 60px`，`bottom: 0`，`z-index: 1` |
| Sidebar 菜单 | `.sidebar` `width` | `272px` |
| Sidebar 菜单 | `.sidebar` `position` | `fixed`，`top: 60px`，`z-index: 2` |
| Sidebar 菜单 | `height` / `overflow` | `height: auto`；`overflow: visible`（无内部滚动） |
| Content Area | `flex` | `1`（占满剩余宽度） |
| Content Area | `padding` | **`32px 24px 24px`**（上 32 = Header 底 → 内容顶；左右下 24；见 `layout-shell.css`） |
| Layout Body | `margin-top` | `60px`（Header 高度） |

---

## 二、Header 顶部导航

### 结构

```
Header (height:60px, padding-left:32px)
├── header-left (flex, gap:24px)
│   ├── Logo (width:132px, height:32px)
│   └── Nav Tabs (flex, gap:24px, overflow:hidden)
└── header-right (flex, gap:24px, flex-shrink:0)
    ├── Search Input (width:200px, height:32px)
    ├── Console Button
    └── User Block (width:160px, height:60px)
```

### Logo

| 属性 | 值 |
|---|---|
| 宽 × 高 | `132px × 32px` |
| SVG viewBox | `0 0 131.947 23.9475` |
| 实现 | 内联 SVG，黑色路径 `fill="var(--fill-0, black)"`，白色文字 `fill="var(--fill-0, white)"` |
| 对齐 | 垂直居中于 Header |

### 顶部导航标签 Nav Tabs

| 属性 | 值 |
|---|---|
| 布局 | `flex`，`gap: 24px`，`overflow: hidden` |
| 文字 | `14px / Regular / #000000`，`line-height: 20px` |
| 下拉箭头容器 | `12 × 12px` |
| 下拉箭头 SVG | `viewBox="0 0 6 4"`，`rotate(180deg)` 使其朝下，`fill: rgba(0,0,0,0.75)` |
| 悬停色 | `color: #1C5CFB` |

### 搜索框

| 属性 | 值 |
|---|---|
| 宽 × 高 | `200px × 32px` |
| 边框 | `1px solid rgba(0,0,0,0.12)`，圆角 `2px` |
| 内边距 | `6px 9px` |
| 占位符 | `13px / rgba(0,0,0,0.34)` |
| 搜索图标 | `16 × 16px`，右对齐 |

### 用户区块（Figma node 4:1294）

绝对定位布局，容器 `width:160px; height:60px; overflow:hidden`

| 元素 | left | top | 尺寸 |
|---|---|---|---|
| 铃铛图标 | `0` | `20px` | `20 × 20px` |
| 通知角标 | `12px` | `14px` | 自适应（`padding: 0 3px`） |
| 用户名 + 箭头行 | `44px` | `20px` | 高 `20px`，`gap: 2px` |

**铃铛图标**：`ic_s_s_bell_filled_20`，填充型，`fill: rgba(#262D34, 0.75)`  
**通知角标**：`background: #F93920`，`border: 1px solid #fff`，`border-radius: 8px`，`font-size: 9px`  
**下拉箭头**：`16 × 16px` SVG，`rotate(180deg)` 朝下，`fill: #262D34`

---

## 三、侧边导航栏 Sidebar

### 容器规格

| 属性 | 值 |
|---|---|
| 宽度 | `272px`（固定） |
| 内边距 | `24px`（四边，`.sidebar`） |
| 背景 | `#FFFFFF`（`.sidebar-column::before` 固定白底） |
| 滚动 | **禁止** 侧栏内部滚动（`overflow: visible`） |
| 定位 | `.sidebar` `position: fixed; top: 60px`；页面滚动时侧栏与白底均不动 |
| 无分隔线 | border / shadow 均已移除 |

### 应用信息区（App Switcher）

| 属性 | 值 |
|---|---|
| 整体宽度 | `224px` |
| 与菜单间距 | `margin-bottom: 40px` |
| 头像尺寸 | `40 × 40px`，`border-radius: 6px`，`overflow: hidden` |
| 头像图源 | `assets/app-avatar.png`（80×80 PNG，《航海王热血航线》路飞角色头像） |
| 头像图层 | 主图（`background-size: cover; background-position: center`）→ `rgba(0,0,0,0.2)` 蒙版 |
| HTML 结构 | `.app-avatar` → `.avatar-bg`（背景图）+ `.avatar-overlay`（蒙版） |
| 应用名 | `14px / Semibold / #000000`，`line-height:20px` |
| AppID | `12px / Regular / rgba(23,26,28,0.45)`，`line-height:16px` |
| 复制图标 | `16 × 16px` SVG，`stroke: #1C5CFB` |
| 切换图标 | `20 × 20px` SVG（`viewBox: 16×16` 内嵌），`stroke: #888D92` |

### 菜单分组头（nav-group）

| 状态 | 属性 | 值 |
|---|---|---|
| 默认 | 宽 × 高 | `224px × 36px` |
| 默认 | 圆角 | `3px` |
| 默认 | 内边距 | `0 8px` |
| 默认 | 图标区 | `20 × 20px`，图标容器与 SVG 均为 `20 × 20px` |
| 默认 | 图标 ↔ 文字间距 | `gap: 12px` |
| 默认 | 文字 | `14px / Semibold / #1C1F23` |
| 默认 | 展开箭头 | `16 × 16px`（Figma `Icon/Default`），`fill: #6B7075` |
| 悬停 / 激活 | 图标色 `--stroke-0` | `#1C5CFB` |
| 悬停 | 背景 | `rgba(0,0,0,0.04)` |

**展开箭头交互**：  
- 展开时 `rotate(0deg)`（朝上 ▲）  
- 折叠时 `rotate(180deg)`（朝下 ▼）  
- 动画：`transition: transform 0.2s ease`

### 菜单子项（nav-item）

| 状态 | 属性 | 值 |
|---|---|---|
| 默认 | 高度 | `36px` |
| 默认 | 圆角 | `3px` |
| 默认 | 文字左边距 | `left: 40px`（与分组图标右边对齐） |
| 默认 | 文字 | `14px / Regular / #1C1F23` |
| 悬停 | 背景 | `rgba(0,0,0,0.04)` |
| **选中** | 背景 | `#EAF5FF` |
| **选中** | 文字 | `14px / Semibold / #1C5CFB` |

### 子菜单折叠动画

```css
.nav-children {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.25s ease;
}
.nav-children.open {
  max-height: 600px;
}
```

---

## 四、内容区 Content Area

| 属性 | 值 |
|---|---|
| 背景 | `#F9F9F9` |
| 弹性 | `flex: 1; min-width: 0` |
| 内边距 | **`32px 24px 24px`**（由 `assets/layout-shell.css` 的 `.content-area` 提供，**禁止**在业务页覆盖为四边 `24px`） |
| 最小高度 | `calc(100vh - 60px)` |
| 业务内容容器 | `.content-area` 内包 `.main`：`max-width: 1072px`；`margin: 0 auto`（各业务 preview 模版） |

**壳层金样**：`preview/LayoutSpec.html` **不覆盖** `.content-area` padding，完全依赖 `layout-shell.css`。生成页面时以该组合为准，勿从业务 preview 反推 padding。

---

## 五、图标规范汇总

| 图标 | 尺寸 | 格式 | 颜色机制 |
|---|---|---|---|
| 导航分组图标（总览/管理等） | `20 × 20px` | SVG 内联 | `stroke="var(--stroke-0)"` |
| 展开/折叠箭头 | `16 × 16px` | SVG 内联 | `fill="var(--fill-0, #6B7075)"` |
| 顶部导航下拉箭头 | 容器 `12×12`，SVG `6×4` | SVG 内联 | `fill: rgba(0,0,0,0.75)` |
| 铃铛通知 | `20 × 20px` | SVG 内联 | `fill: rgba(#262D34, 0.75)` 填充型 |
| 用户区下拉箭头 | `20×20` 容器 / `16×16` SVG | SVG 内联 | `fill: #262D34` |
| 复制 copy | `16 × 16px` | SVG 内联 | `stroke: #1C5CFB` |
| 切换应用 transfer | `20×20` 容器 / `16×16` SVG | SVG 内联 | `stroke: #888D92` |

### 图标染色变量规则

```css
/* 导航分组：默认灰，hover/active 蓝 */
.nav-group            { --stroke-0: #6B7075; }
.nav-group:hover,
.nav-group.active     { --stroke-0: #1C5CFB; }

/* 复制/transfer 图标不在 nav-group 内，直接使用 SVG fallback 值 */
```

---

## 六、间距栅格

| 级别 | 变量 | 值 | 典型用途 |
|---|---|---|---|
| 1 | `--space-1` | `4px` | 图标与文字行内间距 |
| 2 | `--space-2` | `8px` | 菜单项内边距、AppSwitcher gap |
| 3 | `--space-3` | `12px` | 图标与文字 gap（nav-group-inner） |
| 4 | `--space-4` | `16px` | — |
| 5 | `--space-5` | `24px` | 导航 tabs gap、侧边栏内边距、内容区 padding |
| 6 | `--space-6` | `32px` | Header 左侧 padding |
| 7 | `--space-7` | `40px` | AppSwitcher 与菜单列表间距 |

---

## 七、交互行为规范

### 侧边导航

| 行为 | 触发 | 效果 |
|---|---|---|
| 分组展开/折叠 | 点击 group header | 子菜单 `max-height` 0 ↔ 600px，箭头旋转 |
| 子项选中 | 点击 nav-item | 清除所有 `.active`，当前项 `background:#EAF5FF`，文字变蓝加粗 |
| 总览（无子项）直接选中 | 点击总览 group | group 本身 `.active`，内容区更新 |
| 页面滚动 | 滚动内容区 | 侧栏菜单与左侧白色背景 **固定不动**，仅内容区滚动 |

---

## 八、壳层金样与生成约束

> **Skill 入口**：[SKILL.md](../SKILL.md)「壳层金样」「生成工作流与自检」

生成 **任意** B 端页面（HTML / React / Figma 结构）前，壳层 **必须** 对齐本节；业务区块（表格、表单、Tabs 等）再读对应 reference。

### 8.1 金样文件（唯一来源）

| 类型 | 路径 | 用途 |
|------|------|------|
| 规范 | `references/LayoutSpec.md`（本文件） | 尺寸、结构、禁止项 |
| HTML 金样 | `preview/LayoutSpec.html` | **复制** `<header class="header">` 与 `.layout-body`（含侧栏 DOM + 导航脚本） |
| CSS 金样 | `assets/layout-shell.css` | **必须** 引入；`.header` / `.layout-body` / `.sidebar-column` / `.sidebar` / `.content-area` 样式以此为准 |

**禁止**：自写 Semi `Layout` + `Sider` 默认滚动侧栏；从业务 preview 复制壳层；在业务页 CSS 中覆盖 `.sidebar` / `.sidebar-column` 的 `position` / `overflow`。

### 8.2 HTML 拼装顺序

1. 引入 `layout-shell.css`
2. 从 `LayoutSpec.html` 复制 Header + `.layout-body` 侧栏结构（可按业务调整 **菜单选中态**，不改结构与 class）
3. 在 `.content-area` 内仅填充业务 `.main` 内容
4. 业务页 **勿** 重写 `.content-area` 的 `padding-top`（已由 `layout-shell.css` 提供 32px）

业务页若需额外底留白（如固定底栏），仅可追加 `padding-bottom`，例如信息录入页 `padding-bottom: 96px`。

### 8.3 React / Semi 映射

| HTML 金样 | React 要求 |
|-----------|------------|
| `.header` fixed 60px | 顶栏独立组件；`position: fixed; top: 0; z-index: 100`；**勿**包进可滚动 `Layout` |
| `.sidebar-column` + `::before` + `.sidebar` fixed | **勿**用 `Layout.Sider` 默认内部滚动；侧栏 `position: fixed; overflow: visible`；文档流用 272px 占位列 |
| `.content-area` flex:1 | 主内容区随页面滚动；`padding` 与 CSS 金样一致（32/24/24） |
| `.main` max-width 1072px | 业务内容水平居中约束 |

信息录入页优先复用 `PageWithPreviewLayout`（已含壳层与底栏约定）；其他页面若无现成 Layout，按上表实现 fixed 侧栏模式，**禁止**退化为可滚动 Sider。

---

*自动提取自 Figma MCP + HTML 实现归纳 · 节点 13:9326 · 抖音开放平台控制台模板*
