# Assets — 设计规范图形资产

> **Skill 入口**：[SKILL.md](../SKILL.md)「生成必读矩阵」·「图形资产」  
> 生成 HTML / React 页面前对照本文，避免「规范写了 token、页面图标/头像空白」。  
> **目录**：`Games-design-skill/assets/`（`layout-shell.css` · `app-avatar.png` · `icons/*.svg`）  
> **HTML 金样**：`preview/*.html`；图标路径相对 preview 为 `../assets/icons/`  
> **React 图标**：`@douyinfe/semi-icons`（见 §2）

---

## 0. 生成约束（必读）

| 规则 | 说明 |
|------|------|
| **禁止** | 仅写 `semi-icons-brief_stroked` 等 token 而不渲染 SVG / Icon 组件 |
| **HTML 业务图标** | 静态可用 `<img src="../assets/icons/…">`；**JS `innerHTML` 动态插入优先内联 SVG**（见 §2.3） |
| **SVG 编码** | `assets/icons/*.svg` 必须 **UTF-8**；注释优先纯 ASCII；禁止 Latin-1 的 `·` `×` 等（见 §2.3） |
| **禁止** | 使用未登记的 `<use href="#icon-…">` 且页面内无 sprite（会导致空白） |
| **壳层** | Header / 侧栏 DOM 从 [LayoutSpec.html](../preview/LayoutSpec.html) 复制，样式用 `layout-shell.css` |
| **React** | `import { IconX } from '@douyinfe/semi-icons'` |

```html
<img class="meta-icon" src="../assets/icons/icon-mobile-app.svg" width="16" height="16" alt="" aria-hidden="true">
<img class="meta-icon" src="../assets/icons/icon-brief-stroked.svg" width="16" height="16" alt="" aria-hidden="true">
```

```tsx
import { IconMobile, IconBriefStroked, IconPulse } from '@douyinfe/semi-icons';
<IconMobile style={{ color: 'rgba(28,31,35,0.6)' }} />
<IconBriefStroked style={{ color: '#1C5CFB' }} />
```

---

## 1. 文件清单

| 路径 | 类型 | 规范引用 | 说明 |
|------|------|----------|------|
| `layout-shell.css` | CSS | [LayoutSpec.md](../references/LayoutSpec.md) | 壳层 Header / 侧栏 / `.content-area` |
| `app-avatar.png` | 图片 80×80 | LayoutSpec.md §五 · SKILL.md | 侧栏 App 头像（路飞；展示 40×40 + 6px 圆角 + 20% 蒙版） |
| `assets/icons/*.svg` | 矢量图标 | 见 §2 · §4 | 业务页元信息、表格、指标卡、表单控件 |

---

## 2. 图标资产索引

**目录**：`Games-design-skill/assets/icons/`（15 个 svg，均已入库）

| 文件 | 状态 |
|------|------|
| `icon-mobile-app.svg` | ✓ |
| `icon-brief-stroked.svg` | ✓ |
| `icon-pulse.svg` | ✓ |
| `metric-value-rise.svg` | ✓ |
| `icon-semi-logo.svg` | ✓ |
| `icon-action-more.svg` | ✓ |
| `icon-search.svg` | ✓ |
| `icon-chevron-down.svg` | ✓ |
| `icon-calendar.svg` | ✓ |
| `icon-clock.svg` | ✓ |
| `icon-plus.svg` | ✓ |
| `icon-help-circle-16.svg` | ✓ |
| `icon-calendar-clock.svg` | ✓ |
| `icon-export.svg` | ✓ |
| `icon-help-circle.svg` | ✓ |

### 2.0 路径约定

| 引用位置 | 路径 |
|----------|------|
| `preview/*.html` | `../assets/icons/<file>.svg` |
| 生成页与 `assets/` 同级 | `assets/icons/<file>.svg` |
| `references/*.md` 文档链接 | `../assets/icons/<file>.svg` |

### 2.1 图标明细

| id | 文件 | 尺寸 | Semi React | Figma / 场景 |
|----|------|------|------------|--------------|
| `mobile-app` | `assets/icons/icon-mobile-app.svg` | 16×16 | `IconMobile` | 适用 App（CapabilityIntro · InfoQuery intro） |
| `brief-stroked` | `assets/icons/icon-brief-stroked.svg` | 16×16 | `IconBriefStroked` | 能力接入指引链接 |
| `pulse` | `assets/icons/icon-pulse.svg` | 16×16 | `IconPulse` | 当前接入（InfoInput 元信息） |
| `metric-value-rise` | `assets/icons/metric-value-rise.svg` | 48×48 | — | 能力价值指标卡左侧图标 |
| `semi-logo` | `assets/icons/icon-semi-logo.svg` | 22×22 | — | 表格「图片+文本」列；白字标于 `#2E3238` 底（Figma `1446:90540`） |
| `action-more` | `assets/icons/icon-action-more.svg` | 16×20 | `IconMore` | 表格操作列「更多」；`#1C5CFB`（Figma `1442:89933`） |
| `search` | `assets/icons/icon-search.svg` | 16×16 | `IconSearch` | Select 搜索前缀 |
| `chevron-down` | `assets/icons/icon-chevron-down.svg` | 16×16 | — | Select / DatePicker 后缀箭头 |
| `calendar` | `assets/icons/icon-calendar.svg` | 16×16 | `IconCalendar` | DatePicker / 日期范围后缀（`1732:122703`） |
| `clock` | `assets/icons/icon-clock.svg` | 16×16 | `IconClock` | TimePicker 后缀（`1732:122685`） |
| `plus` | `assets/icons/icon-plus.svg` | 16×16 | `IconPlus` | Upload 加号 / Light Button |
| `help-circle-16` | `assets/icons/icon-help-circle-16.svg` | 16×16 | `IconHelpCircle` | 表单标签帮助（`1732:122579`） |
| `calendar-clock` | `assets/icons/icon-calendar-clock.svg` | 16×16 | `IconCalendarClock` | DateTimeRange 后缀（Dashboard 筛选） |
| `export` | `assets/icons/icon-export.svg` | 32×32 | `IconExport` | 数据概览导出按钮画板（含 `#F5F5F5` 底 + 圆角 6px） |
| `help-circle` | `assets/icons/icon-help-circle.svg` | 14×14 | `IconHelpCircle` | 指标卡标题帮助（Dashboard） |

### 2.2 表单控件后缀（InfoInput · 其余无独立 svg）

HTML 可用 `icon-search.svg` / `icon-chevron-down.svg` 作 `background-image`；React **必须**用 Semi：

| 场景 | Semi 组件 | HTML 资产 |
|------|-----------|-----------|
| 搜索选择 | `IconSearch` | `icon-search.svg` |
| Select / 日期后缀 | — | `icon-chevron-down.svg` |
| 时间后缀 | `IconClock` | `icon-clock.svg` |
| 日期后缀 | `IconCalendar` | `icon-calendar.svg` |
| 标签帮助 | `IconHelpCircle` 16×16 | `icon-help-circle-16.svg` |
| 指标卡帮助 | `IconHelpCircle` 14×14 | `icon-help-circle.svg`（Dashboard） |
| 上传加号 | `IconPlus` | `icon-plus.svg` |
| 添加按钮 | `IconPlus` | `icon-plus.svg` |
| 错误态 | `IconAlertCircle` 14×14 | preview 内联 error 圆标 |

### 2.3 SVG 文件编码与 HTML 引用（必读）

#### 问题现象

- 浏览器直接打开 `…/assets/icons/xxx.svg` 报 **`error on line 1 at column 1: Encoding error`**
- 页面里 `<img src="../assets/icons/…">` 或 JS 注入的图标**空白**；壳层内联 SVG、CSS `data:image/svg+xml` 仍正常

#### 根因

SVG 按 **XML** 解析，默认 **UTF-8**。若文件以 **ISO-8859 / Latin-1** 保存，注释里的 `·`（字节 `0xB7`）、`×`（`0xD7`）等会被当成非法 UTF-8，**整文件解析失败**——与是否用 `file://` / HTTP 无关。

#### 文件规范

| 规则 | 说明 |
|------|------|
| **编码** | 全部 `assets/icons/*.svg` 必须为 **UTF-8** |
| **注释** | 优先 **纯 ASCII**（如 `16x16`，勿用 `16×16`；勿在注释写未转 UTF-8 的中文） |
| **禁止** | 以 Latin-1 保存含 `·` `×` 的 Figma 导出注释 |
| **自检** | `file assets/icons/*.svg` 不应出现 `ISO-8859`；浏览器打开 svg URL 应正常渲染 |
| **修复** | 重写首行注释为 ASCII，或整文件另存为 UTF-8 |

```bash
# 仓库根目录执行
file Games-design-skill/assets/icons/*.svg
# 期望：ASCII text 或 UTF-8 text；不应为 ISO-8859
```

#### HTML 引用方式

| 场景 | 推荐 | 说明 |
|------|------|------|
| 静态元信息 `<img>` | `<img src="../assets/icons/…">` **或内联 SVG** | 编码正确时均可；CapabilityIntro · InfoQuery intro 等 |
| **JS 动态插入**（表格行、指标卡 help、表单 suffix） | **内联 SVG 字符串** | 不二次请求文件；`file://` / HTTP 均稳定 |
| Select / 搜索后缀 | CSS `data:image/svg+xml` | InfoInput 金样已用 |
| React 交付 | `@douyinfe/semi-icons` | 不依赖本地 svg 文件 |

**金样对照**：

| 页面 | 图标用法 |
|------|----------|
| [InfoInput.html](../preview/InfoInput.html) | 元信息 + 表单 help / suffix / upload → **内联 SVG** |
| [Modal.html](../preview/Modal.html) | 表格 Logo → **内联 SVG** |
| [InfoQuery.html](../preview/InfoQuery.html) | 表格 Logo / 更多 → `<img>`（资产 UTF-8 后可用；亦可改内联 SVG） |
| [Dashboard.html](../preview/Dashboard.html) | 筛选 / 导出 / 指标 help → 静态 `<img>` |

新增或替换 svg 后：**先浏览器打开文件 URL 确认无 Encoding error**，再接入 preview。

---

## 3. 按页面引用

### 3.1 CapabilityIntro

| 位置 | 资产 | 说明 |
|------|------|------|
| intro / info · 适用 App | `icon-mobile-app.svg` | 与文案 gap 8px |
| intro / info · 指引 | `icon-brief-stroked.svg` | `.meta-link`，蓝色 |
| 能力价值 · 指标卡 | `metric-value-rise.svg` | 48×48 |

详见 [CapabilityIntro.md](../references/CapabilityIntro.md) · [CapabilityIntro.html](../preview/CapabilityIntro.html)。

### 3.2 InfoQuery

| 位置 | 资产 | 说明 |
|------|------|------|
| intro 元信息（单个能力） | 同 CapabilityIntro 元信息图标 | `introMeta: true` |
| 表格 · 图片列 Logo | `icon-semi-logo.svg` | 32×32 容器内 22×22；资产须 UTF-8（§2.3） |
| 表格 · 操作列更多 | `icon-action-more.svg` | `#1C5CFB` 三点（Figma `1442:89933`）；§2.3 |
| 任务状态 / 标签 | 无图标文件 | CSS 圆点 + Tag |

详见 [InfoQuery.md](../references/InfoQuery.md) · [InfoQuery.html](../preview/InfoQuery.html)。

### 3.3 InfoInput

| 位置 | 资产 | 说明 |
|------|------|------|
| 元信息 · 当前接入 | `icon-pulse.svg` | Figma `1732:122551`；金样为**内联 SVG**（§2.3） |
| 元信息 · 指引 | `icon-brief-stroked.svg` | Figma `1732:123496`；金样为**内联 SVG** |
| 数字输入 · 标签帮助 | `icon-help-circle-16.svg` | `fields.numberInput.help: true`；金样 **内联 SVG** |
| 选择 / 搜索 | `icon-chevron-down.svg` · `icon-search.svg` | Select 后缀 / 前缀；金样用 **CSS data URI** |
| 时间 / 日期 / 范围 | `icon-clock.svg` · `icon-calendar.svg` | 控件右 suffix 32px；金样 **内联 SVG** |
| Upload 加号 | `icon-plus.svg` | picture-card 居中；金样 **内联 SVG** |

资产文件供 React / 文档对照；[InfoInput.html](../preview/InfoInput.html) 业务图标已按 §2.3 内联，避免编码与动态加载问题。

详见 [InfoInput.md](../references/InfoInput.md) · [InfoInput.html](../preview/InfoInput.html)。

### 3.4 Dashboard

| 位置 | 资产 | Figma | 说明 |
|------|------|-------|------|
| filter · DateTimeRange 后缀 | `icon-calendar-clock.svg` | `2212:113984` · `322:38975` | 16×16；资产须 UTF-8（§2.3） |
| data-panel · 下载按钮 | `icon-export.svg` | `2212:110979` → `2212:110986` / `2212:110987` | 32×32 完整画板；§2.3 |
| 指标卡 · 帮助 | `icon-help-circle.svg` | `2212:111076` | 14×14；JS 注入时注意 §2.3 |

筛选 Select 后缀复用 `icon-chevron-down.svg` · `icon-search.svg`（同 InfoQuery）。折线图仍为 preview 内 SVG。

详见 [Dashboard.md](../references/Dashboard.md) · [Dashboard.html](../preview/Dashboard.html)。

---

## 4. 样式变量（与 preview 一致）

```css
.meta-icon { width: 16px; height: 16px; flex-shrink: 0; }
.meta-item { display: flex; align-items: flex-start; gap: 8px; }
.meta-link { display: inline-flex; align-items: center; gap: 4px; color: #1c5cfb; }
.metric-icon img { display: block; width: 48px; height: 48px; }
.cell-logo img { display: block; width: 22px; height: 22px; }
.action-more { color: #1c5cfb; }
.metric-help { width: 14px; height: 14px; flex-shrink: 0; }
.btn-export img { display: block; width: 32px; height: 32px; }
.field-date-range .cal-icon { width: 16px; height: 16px; flex-shrink: 0; }
```

---

## 5. 不在本目录、但生成时必须保留

| 资源 | 来源 | 说明 |
|------|------|------|
| 壳层导航图标（Logo、铃铛、菜单、copy 等） | [LayoutSpec.html](../preview/LayoutSpec.html) 内联 SVG | **复制整段壳层 DOM**；见 LayoutSpec.md §五 · §八 |
| Modal 插图 | [Modal.md](../references/Modal.md) §3.1 | CSS 灰底 + 文案，无需图片 |
| Upload 示例图 | [InfoInput.md](../references/InfoInput.md) §3.7 | picture-card 空态 + `+` |
| 应用示例媒体 | [CapabilityIntro.md](../references/CapabilityIntro.md) | 紫底占位文案，无需媒体文件 |
| Dashboard 折线图 | [Dashboard.md](../references/Dashboard.md) | preview 内 SVG 或图表库 |

---

## 6. 按 reference 快速核对

| Reference | 需引入 |
|-----------|--------|
| LayoutSpec | `layout-shell.css` · `app-avatar.png` + 壳层 SVG |
| CapabilityIntro | §3.1 三个 svg |
| InfoQuery | §3.1 + §3.2 |
| InfoInput | §3.3 + §2.2 |
| Dashboard | §3.4（calendar-clock · export · help-circle） |
| Modal | 仅壳层资产 |

---

## 7. Agent 工作流

```
1. LayoutSpec.md §八 → 复制 LayoutSpec.html 壳层 → link layout-shell.css
2. 读本文件 §3（对应页面类型）引入 `assets/icons/*.svg`
3. HTML：静态可用 `<img src="../assets/icons/…">`；**JS 动态图标用内联 SVG**（§2.3）；React：Semi Icon 组件
4. 自检：
   □ app-avatar.png 与 layout-shell.css 同目录可加载
   □ 元信息 / 指标卡 / 表格 Logo·更多 / 表单 suffix均可见
   □ 未使用无 sprite 的 <use href="#icon-…">
   □ `file assets/icons/*.svg` 无 ISO-8859；直接打开 svg URL 无 Encoding error
```

---

*壳层导航图标见 [LayoutSpec.md](../references/LayoutSpec.md) §五（内联 SVG，不在 `icons/`）。*
