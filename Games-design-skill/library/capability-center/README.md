# capability-center 知识库

| 文件 | 说明 |
|------|------|
| `capability-center.json` | 页面数据真源：标题、统计、Tab、推荐区文案、能力卡片列表 |
| `preview.html` | 静态 HTML 金样，浏览器直接打开对照视觉 |
| `SKILL.md` | Agent 修改流程与约束 |

React 组件在 `src/pages/OperationsWorkbenchPage/`，通过 `mockData.ts` 读取 JSON。

## 新增能力卡片

编辑 `capability-center.json`，在 `cards` 数组末尾追加：

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

带默认头像的价值标签：加 `"useDefaultAvatars": true`（对应 `assets/cc/value-tag-avatar-*.png`）。

## 标签字段

- `operationTag` / `statusTag`：标题区角标（互斥展示子能力）
- `subCapabilities`：子能力列表
- `valueTags`：底部价值标签
