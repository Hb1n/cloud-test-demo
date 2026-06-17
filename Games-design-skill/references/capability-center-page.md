# 能力中心页面维护规范（Card 扩展）

本文档用于维护「游戏能力接入 / 能力中心」页面。后续如果能力中心新增功能，统一在该页面新增卡片。

## 预览金样

- HTML 金样：[Capability-center.html](../preview/Capability-center.html)
- 预览截图：[Capability-center.png](../preview/Capability-center.png)（可选，由 HTML 导出）
- 切图目录：`Games-design-skill/assets/cc/`

在浏览器中直接打开 HTML 文件即可对照页面效果；文档内引用截图请使用 `preview/Capability-center.png`。

## 严格生成约束（必读）

**每次生成或迭代能力中心前端页面时，必须以已落地的实现与金样为唯一基准，严格复用既有结构，禁止「按理解重画一页」。**

### 基准对照（生成前必须先读）

1. **React 实现**：`src/pages/OperationsWorkbenchPage/` 下现有页面与组件（以当前仓库代码为准）。
2. **HTML 金样**：[Capability-center.html](../preview/Capability-center.html) 与 [Capability-center.png](../preview/Capability-center.png)。
3. **壳层**：继续使用 `PlatformLayout` + `TopHeader` + `AppSider`（侧栏默认选中能力中心相关菜单），**不得替换为新的导航结构或自定义壳层**。
4. **布局规范（壳层必读）**：[LayoutSpec.md](./LayoutSpec.md) — 页面还原时，**顶部导航与左侧边栏的布局、尺寸、定位与交互须严格遵循该文档**（含 [LayoutSpec.html](../preview/LayoutSpec.html) 与 `assets/layout-shell.css` 金样）。

### 壳层布局约束（侧边栏 + 顶部导航）

页面还原或迭代时，**仅业务内容区**（页头、筛选、推荐区、卡片网格）以能力中心金样为准；**壳层**须与 [LayoutSpec.md](./LayoutSpec.md) **严格对齐**，不得偏离：

| 壳层区域 | 须遵循 LayoutSpec 的要点 |
|----------|--------------------------|
| 顶部导航 | Header 固定 `60px` 高、`z-index: 100`；Logo `132×32`；Nav Tabs `gap: 24px`；搜索框 `200×32`；用户区块 `160×60` 及铃铛/角标/下拉箭头规格 |
| 左侧边栏 | 占位列 `272px` + 固定白底 + `.sidebar` `position: fixed; top: 60px; overflow: visible`（**禁止**侧栏内部滚动）；App Switcher、菜单分组/子项尺寸、选中态 `#EAF5FF` / `#1C5CFB` |
| 内容区壳层 | `.content-area` `padding: 32px 24px 24px`；`flex: 1`；背景 `#F9F9F9` — **禁止**在业务页覆盖为四边 `24px` |

**禁止**：自写 Semi `Layout` + `Sider` 默认滚动侧栏；从业务 preview 反推壳层 padding/定位；在业务 CSS 中覆盖 `.sidebar` / `.sidebar-column` 的 `position` / `overflow`。详见 LayoutSpec 第八节「壳层金样与生成约束」。

### 必须保持一致的页面元素

以下区块的结构、层级、样式 token 与交互逻辑须与基准页**完全一致**，不得擅自删改、合并或重排：

| 区域 | 说明 |
|------|------|
| 顶部导航 | `TopHeader`：Logo、主导航、搜索、控制台、用户区 — **布局规格严格参照 [LayoutSpec.md](./LayoutSpec.md) 第二节** |
| 左侧边栏 | `AppSider`：应用切换、能力中心菜单与选中态 — **布局规格严格参照 [LayoutSpec.md](./LayoutSpec.md) 第三节** |
| 页头区 | 「游戏能力接入」标题 + 100/100 数据行 +「最近访问」块 |
| 筛选区 | 运营/研发/其他 Tab + 分类 Pill +「查看全部 / 只看未接入」切换 |
| 推荐区 | 蓝渐变底 +「推荐接入」标题切图 + 首行 3 张推荐卡 |
| 卡片网格 | 三列 `CapabilityCard` 布局及卡片内部信息/标签规则 |
| 切图与资产 | 统一走 `Games-design-skill/assets/cc/`，经 `assets.ts` 引用 |

### 允许的变更范围

仅在**不改变上述骨架**的前提下，可做：

- 在 `mockData.ts` 的 `CAPABILITY_CARDS` **追加**新能力卡片数据；
- 为新卡片配置 `capabilityType`、`categories`、各类标签（见下文「标签配置规则」）；
- 若新卡片需要新切图，在 `assets/cc` 增补资源并在 `assets.ts` 导出（不改动既有切图用法）。

### 禁止事项

- 禁止重写或替换顶部导航、侧栏、页头、筛选栏、推荐区容器等**已有页面骨架**。
- 禁止顶部导航、侧栏在尺寸、定位、滚动行为上偏离 [LayoutSpec.md](./LayoutSpec.md)（如侧栏可滚动、Header 非 fixed 60px、content-area padding 被业务页覆盖等）。
- 禁止为「新增一张卡」而新建另一套页面布局、样式体系或平行组件。
- 禁止使用 Figma 临时 URL 或未归档到 `assets/cc` 的图片资源。
- 禁止破坏卡片互斥规则（标题角标 vs 子能力）、推荐区与下方网格对齐、三列响应式断点等行为。

### Agent / 生成流程建议

```
1. 先读 LayoutSpec.md + LayoutSpec.html，确认 TopHeader / AppSider 壳层与布局规范一致
2. 打开 Capability-center.html / Capability-center.png 与 OperationsWorkbenchPage 源码对照
3. 确认壳层（顶栏/侧栏按 LayoutSpec）、页头、筛选、推荐区、卡片组件均未偏离基准
4. 仅在 mockData.ts 追加 CapabilityCardData（及必要的新切图）
5. npm run build + 视觉对照金样截图做回归
```

## 页面入口与核心文件

- 页面组件：`src/pages/OperationsWorkbenchPage/OperationsWorkbenchPage.tsx`
- 卡片组件：`src/pages/OperationsWorkbenchPage/CapabilityCard.tsx`
- 样式文件：`src/pages/OperationsWorkbenchPage/OperationsWorkbenchPage.module.scss`
- 数据源：`src/pages/OperationsWorkbenchPage/mockData.ts`
- 类型定义：`src/pages/OperationsWorkbenchPage/types.ts`
- 资源常量：`src/pages/OperationsWorkbenchPage/assets.ts`

## 资产目录（统一放到 assets/cc）

能力中心页面专用切图统一放在：

- `Games-design-skill/assets/cc/`

当前已归档资产：

- `Games-design-skill/assets/cc/recommended-title@2x.png`
- `Games-design-skill/assets/cc/recent-block-decor.svg`
- `Games-design-skill/assets/cc/value-tag-avatar-1.png`
- `Games-design-skill/assets/cc/value-tag-avatar-2.png`
- `Games-design-skill/assets/cc/value-tag-avatar-3.png`

说明：

- 页面代码只通过 `src/pages/OperationsWorkbenchPage/assets.ts` 引用这些资产。
- 后续新增切图时，先放入 `assets/cc`，再在 `assets.ts` 中新增常量并统一导出。

## 新增能力卡片（能力中心新增功能）

能力中心新增功能时，按以下步骤新增卡片：

1. 在 `src/pages/OperationsWorkbenchPage/mockData.ts` 的 `CAPABILITY_CARDS` 追加一条 `CapabilityCardData`。
2. 根据功能类型设置 `capabilityType`：`ops | dev | other`。
3. 设置分类 `categories`（至少包含 `all`，可叠加 `recommended | acquisition | retention | revenue`）。
4. 根据业务状态配置标签（见下方「标签配置规则」）。

示例：

```ts
{
  id: 'new-feature-id',
  title: '新能力名称',
  description: '能力说明文案',
  capabilityType: 'ops',
  categories: ['all', 'recommended', 'revenue'],
  connected: false,
  statusTag: '未接入',
  valueTags: [
    { text: '转化提升 10%' },
    { text: '社交裂变' },
  ],
}
```

## 标签配置规则（支持不同标签）

`CapabilityCardData` 支持三类标签能力：

1. 标题区标签
   - `operationTag`：如「运营」
   - `statusTag`：如「未接入」
2. 子能力标签
   - `subCapabilities: string[]`
3. 底部价值标签
   - `valueTags: Array<{ text: string; avatars?: string[] }>`

互斥规则：

- 标题区存在 `operationTag` 或 `statusTag` 时，不展示 `subCapabilities`（已在组件内处理）。

推荐实践：

- 纯文本价值标签：`{ text: '提升收入 12%' }`
- 带头像组标签：`{ text: '留存大幅提升', avatars: [avatar1, avatar2, avatar3] }`

## 回归检查清单

- **壳层与 LayoutSpec 一致**：顶栏、侧栏的尺寸、定位、交互严格符合 [LayoutSpec.md](./LayoutSpec.md)（对照 [LayoutSpec.html](../preview/LayoutSpec.html)）。
- **页面骨架与金样一致**：顶栏、侧栏、页头、筛选、推荐区、卡片网格未偏离 [Capability-center.html](../preview/Capability-center.html)。
- 本次变更若为新增能力，**仅**在 `CAPABILITY_CARDS` 增加数据，未改动既有布局与壳层组件。
- 卡片在三列网格下是否对齐（窄屏是否按 2 列/1 列自动换行）。
- 标题区标签与子能力互斥是否生效。
- 价值标签是否超长省略并可 tooltip 查看（如有）。
- 切图路径是否都来自 `Games-design-skill/assets/cc`，无 Figma 临时 URL。
- 页面构建通过：`npm run build`。

