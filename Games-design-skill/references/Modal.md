# Modal — 弹窗

> **设计来源**：[互动游戏_平台产品-UI-Kit](https://www.figma.com/design/8ca5RrQYK8JRq7ZNJh3xsA/%E4%BA%92%E5%8A%A8%E6%B8%B8%E6%88%8F_%E5%B9%B3%E5%8F%B0%E4%BA%A7%E5%93%81-UI-Kit)  
> - 弹窗模块节点 `916:50798`  
> - **插图样式一** `1150:15525` · **插图样式二** `1150:15778`  
> **设计体系**：Semi Design（抖音开放平台 B 端）  
> **页面用途**：业务弹窗（确认框、选择框、引导弹层、表单单屏提交）。生成时 **必读** [LayoutSpec.md](./LayoutSpec.md) §八（若页面含控制台壳层）+ 本文件。  
> **HTML 预览**：[`../preview/Modal.html`](../preview/Modal.html)（`applyModalShowcaseConfig` · `MODAL_SHOWCASE_CONFIG` · `openPlatformModal`）  
> **更新日期**：2026-06-10

---

## 0. 三维度体系

弹窗由 **尺寸 · 内容 · 确认提示** 三个维度共同定义，生成或还原时按顺序判定：

```
PlatformModal
├── 1. 尺寸     small | medium | large     ← 智能选宽 resolveModalSize
├── 2. 内容
│   ├── 2.1 插图样式     新功能引导 / 提醒
│   ├── 2.2 信息展示     纯文本 · 表格
│   └── 2.3 信息输入     输入框 · 选择框 · 单选
└── 3. 确认提示   常规提示 · 风险提示（标题+关闭必填）
```

| 维度 | 核心规则 |
|------|----------|
| **1. 尺寸** | `small` / `medium` / `large` 三档（448 / 684 / 920 px），优先按 **智能选宽** 判定 |
| **2. 内容** | 分 **2.1 插图**、**2.2 信息展示**、**2.3 信息输入** 三类 |
| **3. 确认提示** | **标题 + 右上角关闭必填**；**常规提示**（取消+蓝色确定）· **风险提示**（取消+红色主按钮） |

### 组件

统一使用 **`PlatformModal`**（含表格、表单、插图、确认/删除等场景）。

---

## 1. Design Tokens

| Token | 值 | 用途 |
|-------|-----|------|
| `--modal-width-small` | **448px** | Small |
| `--modal-width-medium` | **684px** | Medium |
| `--modal-width-large` | **920px** | Large |
| `--modal-padding-x` | **24px** | 水平内边距 |
| `--modal-title-body-gap` | **24px** | 标题 → 正文（说明文案下 **8px**） |
| `--modal-footer-height` | **80px** | 底栏高度 |
| Footer 按钮间距 | **8px** | 取消 / 确定 |
| 正文 → 按钮区 | **24px** | 仅 footer 上 padding，禁止双重 24px |
| 危险主按钮 | `#F93920` | 删除等风险操作 |

> 宽度 **仅** 448 / 684 / 920。

---

## 2. 维度一：尺寸

包含 **small / medium / large** 三个尺寸（448 / 684 / 920 px）。

### 宽度限制规则（智能选宽）

**优先用最小的**；仅当内容需要更多空间时再升档：

| 优先级 | 条件 | 档位 |
|--------|------|------|
| 默认 | 无下列升档条件 | **small**（448px） |
| 升档 1 | 文字行数 **> 3** | **medium**（684px） |
| 升档 2 | 含 **表格** | 至少 **medium**（684px） |
| 升档 3 | 表格且 **隐藏列 > 3** | **large**（920px） |

> 含表格时直接适用升档 2；若同时满足隐藏列 > 3，则升至 large。文字行数规则与表格规则独立判定，取 **更大** 档位。

`resolveModalSize` 实现：

```javascript
resolveModalSize({ textLines, hasTable, hiddenColumnCount })

function resolveModalSize(ctx = {}) {
  const lines = ctx.textLines ?? ctx.textBlocks;
  if (ctx.hasTable) {
    return ctx.hiddenColumnCount > 3 ? 'large' : 'medium';
  }
  if (lines > 3) return 'medium';
  return 'small'; // 优先最小
}
```

显式传入 `size` 时 **优先于** `sizeContext`。

---

## 3. 维度二：内容

内容大致分为 **3 类**：

### 3.1 插图样式（新功能引导 / 提醒）

用于新功能上线引导、重要提醒等场景。

#### 样式一 `1150:15525`（`EmphasisStyle1Content`）

```
┌──────────────────────────────┐
│ [顶图 200px 灰底]        [×] │  ← 关闭 top 17px / right 16px
│  主标题文案（20px 居中）      │
│  · 辅助文案列表（14px）       │
├──────────────────────────────┤
│           取消    确定        │
└──────────────────────────────┘
```

| 项 | 值 |
|----|-----|
| 宽度 | **448px**（small） |
| 顶图 | 高 **200px**，底 `rgba(0,0,0,0.05)`，占位「插图 21:9（固定）」 |
| 关闭 | `closableOnly`；**无** header；关闭叠在顶图右上 |
| 标题 | body 内 **20px / 28px** 居中 `#1C1C23` |
| 列表 | 宽 **378px**；`list-disc`；14px `rgba(28,28,35,0.8)`；项间距 **4px** |
| 标题→列表 | **16px** |

#### 样式二 `1150:15778`（`EmphasisStyle2Content`）

```
┌──────────────────────────────┐
│      主标题文案           [×] │  ← header 居中标题 20px
│  副标题文案 + 描述段落 × N      │
│  [插图 16:9  398×230 圆角8]  │
├──────────────────────────────┤
│           取消    知道了        │
└──────────────────────────────┘
```

| 项 | 值 |
|----|-----|
| 宽度 | **448px**（small） |
| Header | 标题 **20px** 居中 + 右侧关闭；`padding-top 24px` |
| 正文 | `px 24px`；区块间距 **16px** |
| 副标题 | 14px Semibold `rgba(28,31,35,0.8)` |
| 描述 | 14px Regular 同色 |
| 插图 | **398×230**，圆角 **8px**，底 `#F2F2F2`；占位「插图 16:9（建议）」 |
| 底栏 | **取消** + **知道了**（主色） |

### 3.2 信息展示（纯文本 · 表格）

用于阅读、确认、选择数据等 **只读或选择** 场景。

```
┌──────────────────────────────┐
│ 标题                      [×]│
│ 副文案（标题下 **8px**）      │
│ 正文 / 表格                  │
│                    [分页 →]  │
├──────────────────────────────┤
│           取消    确定        │  ← 底栏可选
└──────────────────────────────┘
```

| 类型 | 组件 / 实现 | 要点 |
|------|-------------|------|
| **纯文本** | 正文 slot | `bodyPadding: true`；短文案默认 small |
| **表格** | `TableSelectContent` | 默认 **medium**（684px）；样式遵循 [InfoQuery.md](./InfoQuery.md) §2.7 · §2.10；**六列平均分布**；筛选框固定 **216×32px**；**任务状态**列 `StatesTag`、**标签**列 `Tag Small`（默认五态色值统一，列表内标签列默认 `succeed` / 「Tag」）；**标题→副文案 8px**、**副文案→筛选 24px**、**筛选→列表 16px**、**列表→分页 16px** |

### 3.3 信息输入（输入框 · 选择框 · 单选）

用于弹窗内短表单填写：

```
┌──────────────────────────────┐
│ 标题                      [×]│
│ 左标签  输入框 / 选择框 / 单选 │
├──────────────────────────────┤
│           取消    确定        │
└──────────────────────────────┘
```

| 控件 | 组件 | 要点 |
|------|------|------|
| 输入框 | `SmallFormModalContent` | 左标签 **145px**，字重 **400**；单行控件宽度随弹窗尺寸自适应，右缘与底栏按钮对齐 |
| 选择框 | `Form.Select` | 样式同 [InfoQuery.md](./InfoQuery.md) §2.5（灰底、下拉箭头）；**单行时宽度随弹窗尺寸自适应**，右缘与底栏按钮对齐 |
| 单选 | `Form.RadioGroup` | 与首行控件对齐 |

---

## 4. 维度三：确认提示

### 4.1 必填：标题 + 右上角关闭

- 所有弹窗 **必须** 有标题与右上角关闭按钮
- 插图样式一例外：关闭叠在顶图内（`closableOnly`），标题在 body 内

### 4.2 常规提示

用户需确认的操作，底栏 **取消 + 确定**，主按钮 **蓝色** primary：

```tsx
footerActions={createModalFooterActions(
  MODAL_INTERACTION_PRESET.confirm,
  { onCancel: close, onConfirm: onSubmit },
)}
```

### 4.3 风险提示

删除、下架等风险操作，底栏 **取消 + 危险主按钮**，主按钮 **红色**（`#F93920`）：

```tsx
footerActions={createModalFooterActions(
  MODAL_INTERACTION_PRESET.danger,
  { onCancel: close, onConfirm: onDelete },
)}
```

### 确认提示速查

| 类型 | Preset | 底栏 | 主按钮 |
|------|--------|------|--------|
| **常规提示** | `confirm` | 取消 + 确定 | 蓝色 primary |
| **风险提示** | `danger` | 取消 + 删除 | **红色 danger** |

---

## 5. PlatformModal Props

| Prop | 规则 |
|------|------|
| `size` | 显式指定时优先于 `sizeContext` |
| `sizeContext` | 智能选宽：`{ textLines, hasTable, hiddenColumnCount }` |
| `title` | 必填（插图样式一标题在 body 内） |
| `closableOnly` | 仅插图样式一 |
| `bodyPadding` | 纯文本 `true`；表格/表单/插图 `false` |
| `footerActions` | `confirm` / `danger` 预设 |
| `titleCenter` | 插图样式二 |

---

## 6. 生成 React 代码

```tsx
import {
  PlatformModal,
  TableSelectContent,
  resolveModalSize,
  createModalFooterActions,
  MODAL_INTERACTION_PRESET,
} from '@/components/PlatformModal';

// 2.2 表格 + 智能选宽 + 标准确认
<PlatformModal
  visible={open}
  onCancel={close}
  title="选择游戏站道具"
  sizeContext={{ hasTable: true }}
  bodyPadding={false}
  footerActions={createModalFooterActions(
    MODAL_INTERACTION_PRESET.confirm,
    { onCancel: close, onConfirm: submit },
  )}
>
  <TableSelectContent description="当前仅展示已生效的游戏站道具" />
</PlatformModal>

// 4.3 风险提示（红钮）
footerActions={createModalFooterActions(
  MODAL_INTERACTION_PRESET.danger,
  { onCancel: close, onConfirm: onDelete },
)}
```

---

## 7. 三维度决策流程

```
1. 判定内容类型
   ├─ 新功能引导/提醒 → 2.1 插图
   ├─ 阅读/选数据     → 2.2 信息展示（纯文本 or 表格）
   └─ 填写表单         → 2.3 信息输入
        ↓
2. 智能选宽（优先最小）
   ├─ 默认 small
   ├─ 文字行数>3      → medium
   ├─ 含表格           → 至少 medium
   └─ 表格隐藏列>3     → large
        ↓
3. 判定确认提示
   ├─ 常规操作 → confirm（蓝色确定）
   └─ 风险操作 → danger（红色主按钮）
```

---

## 8. 禁止项

- 宽度非 448 / 684 / 920
- 正文底 + footer 顶双重 24px
- 表格弹窗用 Small
- 无标题或无关闭按钮
- 风险操作用蓝色主按钮

---

## 9. 自检清单

### 尺寸

- [ ] 宽度仅 448 / 684 / 920
- [ ] 已用 `resolveModalSize` 或显式 `size`
- [ ] 优先 small；文字行数 > 3 → medium
- [ ] 含表格 ≥ medium；表格隐藏列 > 3 → large

### 内容

- [ ] **2.1** 插图：样式一 `1150:15525` 顶图 200px + 关闭 17/16px；样式二 `1150:15778` 居中标题 + 16:9 配图
- [ ] **2.2** 展示：纯文本 bodyPadding；表格符合列表规范（§2.7 行高/表头/分页）；任务状态 `StatesTag`、标签 `Tag Small` 五态色值与列表一致
- [ ] **2.3** 输入：标签 400 字重、左标签 145px

### 确认提示

- [ ] 标题 + 关闭必填
- [ ] 常规提示：取消 + 蓝色确定
- [ ] 风险提示：取消 + 红色主按钮

---

*字段级组件见 semi-ui.skill；页面级长表单见 [InfoInput.md](./InfoInput.md)。*
