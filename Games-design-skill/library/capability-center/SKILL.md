---
name: capability-center
description: >-
  抖音开放平台「游戏能力接入 / 能力中心」页面知识库：JSON 数据 + HTML 预览金样。
  修改 capability-center.json 追加能力卡片；React 在 src/pages/OperationsWorkbenchPage/。
  壳层见 navigation.skill。
---

# capability-center — 能力中心知识库

> **数据真源**：`capability-center.json`  
> **视觉金样**：`preview.html`  
> **禁止**仅凭 MD 或截图重生成整页；改 JSON + 对照 preview 即可。

## 文件

| 文件 | 用途 |
|------|------|
| `capability-center.json` | 页面文案、筛选 Tab、能力卡片数据 |
| `preview.html` | 静态 HTML 金样（浏览器直接打开） |

React 实现在 `src/pages/OperationsWorkbenchPage/`（`mockData.ts` 自动读取 JSON）。

## 何时使用

- 能力中心**新增功能** → 在 JSON `cards` 追加条目
- 调整卡片标签、分类、接入状态
- 对照 Figma `2236:36558` 做视觉回归

## 严格约束

- **不得**重写顶栏、侧栏、页头、筛选、推荐区骨架（改 `src/pages/OperationsWorkbenchPage/` 组件时才涉及）
- 常见需求**只改 JSON**，不改 React 结构
- 切图只走 `Games-design-skill/assets/cc/`
- 禁止使用 Figma 临时 URL

## 新增卡片（最常见）

编辑 `capability-center.json` → `cards` 数组：

```json
{
  "id": "new-feature-id",
  "title": "新能力名称",
  "description": "能力说明文案",
  "capabilityType": "ops",
  "categories": ["all", "recommended"],
  "connected": false,
  "statusTag": "未接入",
  "valueTags": [{ "text": "转化提升 10%" }]
}
```

- `capabilityType`：`ops` | `dev` | `other`
- `categories`：至少含 `all`，可叠加 `recommended` | `acquisition` | `retention` | `revenue`
- 带头像价值标签：`{ "text": "留存大幅提升", "useDefaultAvatars": true }`
- `operationTag` / `statusTag` 与 `subCapabilities` 互斥（组件内处理）

## 修改后自检

1. `npm run build`
2. 浏览器打开 `preview.html` 对照
3. 走 [capability-center-page.md](../../templates/capability-center-page.md) 回归清单

## 业务工程

```ts
import { OperationsWorkbenchPage } from '@/pages/OperationsWorkbenchPage';
```
