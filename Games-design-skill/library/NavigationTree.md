# Navigation Tree — 抖音开放平台控制台目录树

> **用途**：作为「站点信息架构」单一事实源（Single Source of Truth）。  
> 大模型在生成页面前，**先对照本目录树判断**：
> 1. 该需求是 **新页面** 还是 **既有页面迭代**；
> 2. 功能应**挂在哪个一级分组 / 二级菜单**之下；
> 3. 该位置默认应套用 `references/` 下的**哪个页面模版**。
>
> 来源：侧栏导航截图 + [`../references/LayoutSpec.md`](../references/LayoutSpec.md)（侧栏 `nav-group` / `nav-item` 结构）。  
> 配套机器可读版本：[`NavigationTree.json`](./NavigationTree.json)。

---

## ⚠️ 壳层强制规范（顶栏 / 侧栏）

> **任何页面的顶栏与侧栏都必须严格遵守布局规范，不得自由发挥。**

- **唯一权威壳层来源**：[`../preview/LayoutSpec.html`](../preview/LayoutSpec.html)。生成新页面时，顶栏 + 侧栏的 **DOM 结构、官方 logo SVG、各分组官方图标、`app-switcher`、`nav-arrow` 展开/折叠 JS** 必须直接复制自该文件，逐字保留。
- **样式来源**：必须引用 `../assets/layout-shell.css`，禁止在页面内重写壳层样式（业务区样式不受限）。
- **禁止破损资源**：壳层图标一律使用 **内联 SVG**（与权威壳层一致）；禁止 `<img src="...assets/...svg">` 等外链图标（这些文件可能不存在，会导致破图）。
- **允许修改的仅限**：① `nav-item` 文案/链接（按本目录树填充）；② 当前页对应的 **激活项**（`nav-item.active`）；③ `nav-children.open` 展开状态；④ `data-href` 跨页跳转。
- **激活语义**：遵循权威 JS——子项选中时仅 `nav-item.active` 高亮，父级 `nav-group` 不加 `active`；`总览`（无子项）点击时高亮 `nav-group.active`。
- 自检：生成后必须确认壳层无外链破图、`layout-shell.css` 已引用、7 个一级分组与官方图标齐全、箭头方向与展开态一致。

---

## 0. 使用方式（决策流程）

```
需求 / PRD
   │
   ├─ 1. 在「目录树」中按功能语义查找匹配的二级菜单(leaf)
   │
   ├─ 命中 leaf ──────────────► 既有页面【迭代】
   │        └─ 在该 leaf 对应路由上修改，沿用其 template，复用现有字段/Tab
   │
   └─ 未命中 leaf
            ├─ 命中某个一级分组(group) ─► 【新页面】，挂到该 group 下，新增 leaf
            │        └─ 按 group 的「默认模版」选型；如属能力类，多为 CapabilityIntro
            │
            └─ 任何 group 都不合适 ─────► 【新分组 + 新页面】，需人工确认后再新增 group
```

**判定要点**

| 判断 | 规则 |
|------|------|
| 迭代 vs 新页面 | 能在二级菜单(leaf)层找到**同一功能域**的条目 → 迭代；否则新页面 |
| 归属分组 | 按功能性质匹配 group：数据分析→Data；平台能力/变现→Capability/Commercialization；配置→Development/Settings；列表治理→Management |
| 模版选型 | 优先用 leaf 的 `template`；新页面用所属 group 的「默认模版」，再对照对应 `references/*.md` 细分变体 |
| 新增分组 | 仅当功能与所有现有 group 均不相关时；**必须先与用户确认**，不可擅自新增一级分组 |

---

## 1. 目录树（L1 分组 → L2 菜单）

```
抖音开放平台控制台
├── Overview  总览                         [single page · 无子项]
│
├── Management  管理
│   ├── Version Management  版本管理
│   └── User Feedback  用户反馈
│
├── Data  数据
│   ├── User Analysis  用户分析
│   ├── Growth Analysis  增长分析
│   ├── Performance Analysis  性能分析
│   └── Custom Analysis  自定义分析
│
├── Capability  能力
│   ├── Capability Center  能力中心
│   ├── Publisher Program  发行人计划
│   ├── Ad Delivery  广告投放
│   ├── Coin Incentive  金币激励
│   └── Customer Service  客服
│
├── Commercialization  商业化
│   ├── Traffic Monetization  流量主
│   └── Virtual Payment  虚拟支付
│
├── Development  开发
│   ├── Development Settings  开发设置
│   └── Cloud Testing Service  云测试服务
│
└── Settings  设置
    └── Basic Settings  基础设置
```

---

## 2. 菜单明细（含路由 / 默认模版 / 说明）

> `template` 取值：`Dashboard` / `InfoQuery` / `CapabilityIntro` / `InfoInput` / `Modal`（详见 `../references/`）。

### L1 · Overview 总览
| 菜单 | id | route | template | 说明 |
|------|----|-------|----------|------|
| 总览 | `overview` | `/console/overview` | Dashboard | 单页，无子项；全局经营概览与关键指标 |

### L1 · Management 管理
| 菜单 | id | route | template | 说明 |
|------|----|-------|----------|------|
| 版本管理 | `version-management` | `/console/management/version` | InfoQuery | 版本列表、提审、上下线记录 |
| 用户反馈 | `user-feedback` | `/console/management/feedback` | InfoQuery | 反馈工单列表、处理状态 |

### L1 · Data 数据
| 菜单 | id | route | template | 说明 |
|------|----|-------|----------|------|
| 用户分析 | `user-analysis` | `/console/data/user` | Dashboard | 用户规模、留存、画像看板 |
| 增长分析 | `growth-analysis` | `/console/data/growth` | Dashboard | 拉新、转化、渠道增长看板 |
| 性能分析 | `performance-analysis` | `/console/data/performance` | Dashboard | 性能指标看板、机型档位筛选、性能预警 |
| 自定义分析 | `custom-analysis` | `/console/data/custom` | Dashboard | 自定义指标 / 维度看板 |

### L1 · Capability 能力
| 菜单 | id | route | template | 说明 |
|------|----|-------|----------|------|
| 能力中心 | `capability-center` | `/console/capability/center` | CapabilityIntro | 平台能力总览与各能力介绍/接入（如「全端经营-App」） |
| 发行人计划 | `publisher-program` | `/console/capability/publisher` | CapabilityIntro | 发行人计划能力介绍与接入 |
| 广告投放 | `ad-delivery` | `/console/capability/ad-delivery` | CapabilityIntro | 广告投放能力介绍与配置 |
| 金币激励 | `coin-incentive` | `/console/capability/coin-incentive` | CapabilityIntro | 金币激励能力介绍与配置 |
| 客服 | `customer-service` | `/console/capability/customer-service` | InfoQuery | 客服能力 / 会话与工单管理 |

### L1 · Commercialization 商业化
| 菜单 | id | route | template | 说明 |
|------|----|-------|----------|------|
| 流量主 | `traffic-monetization` | `/console/commercialization/traffic` | CapabilityIntro | 流量主开通、广告位与收益 |
| 虚拟支付 | `virtual-payment` | `/console/commercialization/virtual-payment` | Dashboard | 虚拟支付数据、支付能力配置 |

### L1 · Development 开发
| 菜单 | id | route | template | 说明 |
|------|----|-------|----------|------|
| 开发设置 | `development-settings` | `/console/development/settings` | InfoInput | 开发密钥、回调、SDK 等配置表单 |
| 云测试服务 | `cloud-testing-service` | `/console/development/cloud-testing` | CapabilityIntro | 云真机测试能力介绍与任务管理 |

### L1 · Settings 设置
| 菜单 | id | route | template | 说明 |
|------|----|-------|----------|------|
| 基础设置 | `basic-settings` | `/console/settings/basic` | InfoInput | 应用基础信息、成员与权限配置表单 |

---

## 3. 已实现页面映射（现状）

> 用于判断「迭代」时定位现有文件。页面位于 `workspace/`。

| 现有文件 | 归属 group → leaf | 类型 | 模版 |
|----------|-------------------|------|------|
| `workspace/性能预警与看板优化.html` | Data → Performance Analysis | 既有页迭代 | Dashboard |
| `workspace/全端经营-能力介绍.html` | Capability → Capability Center | 既有页（能力介绍 Tab） | CapabilityIntro · 单层 Tab |
| `workspace/全端经营-App接入.html` | Capability → Capability Center | 既有页（App 接入 Tab） | CapabilityIntro + Steps + InfoInput |

> 同一二级菜单下可存在多个能力子页（通过 Line Tab / 面包屑区分），新增同能力的功能优先以 **Tab/子页** 形式挂入，而非新增 leaf。

---

## 4. 维护约定

1. 新增 / 调整菜单时，**同时更新** 本文件与 [`NavigationTree.json`](./NavigationTree.json)。
2. `id` 全局唯一，使用 **kebab-case 英文**；`route` 与 `id` 层级保持一致。
3. 新增一级分组属重大信息架构变更，需在确认清单中显式标注并经用户确认。
4. 截图为信息架构来源；如侧栏实际顺序/命名调整，以最新截图为准并回写本文件。
