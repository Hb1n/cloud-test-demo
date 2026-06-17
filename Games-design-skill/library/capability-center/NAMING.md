# Skill 命名规范

## 格式

```
{kebab-case-topic}.skill/
```

| 部分 | 规则 | 示例 |
|------|------|------|
| 主题 | 小写英文，单词用 `-` 连接；优先**应用场景**而非布局实现 | `info-entry`、`navigation`、`semi-ui` |
| 后缀 | 固定 `.skill` | 表示 Cursor Agent Skill 目录 |
| 禁止 | 无后缀、`skills` 复数、驼峰、纯布局名 | ~~`page-with-preview`~~、~~`semi-ui-skills`~~ |

## 放置位置

| 类型 | 路径 | 说明 |
|------|------|------|
| **页面模版 / 结构** | `templates/{name}.skill/` | 布局壳、多区域编排 |
| **组件 / 跨页能力** | `{name}.skill/`（仓库根下） | 如 Semi 组件指南 |

## 当前 Skill 清单

| 目录 | 类型 | 触发示例 |
|------|------|----------|
| `templates/navigation.skill` | 模版 | `@navigation.skill`、平台顶栏侧栏 |
| `templates/info-entry.skill` | 模版 | `@info-entry.skill`、信息录入、单一信息模块 |
| `templates/modal.skill` | 模版 | `@modal.skill`、弹窗、Modal、Popconfirm |
| `semi-ui.skill` | 组件 | `@semi-ui.skill`、Form.Upload |

## Cursor 加载

源码仅在 `Game-design-system-skill/`。`.cursor/skills/{name}.skill` 为**同名**符号链接。

对话中 `@` 引用请使用**目录全名**（如 `@navigation.skill`），与文件夹一致。
