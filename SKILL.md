---
name: pm-proto-prd-pin
description: 为任意 HTML 原型一键植入「交互式 PRD 打点标注器与多版本规约生成系统」。严格遵循标准实现：全球 4 语言架构 (中/英/日/韩: zh-CN, en, ja, ko)、REST API 真实落盘与本地 JS 双层持久化、在原型元素上高精度十字准星打点标注 (打标时自动收起右侧抽屉)、智能展开弹窗/Tab精准定位、三态抽屉 (400px全展开 / 56px标号竖条半收起 / 0px全收起)、左侧双按钮控制组 (全收起+半收起朝内区隔)、纯白底色 Mermaid 矢量图表引擎与动态防截断渲染、真正交互式可视化表格直编 (零管道符如同 Excel 点击单元格打字并支持一键增删行列与 Shift+Enter 换行)、纯净无框逐行即时文档工作台 (带 caretRangeFromPoint 精准光标落点算法、工具栏焦点持续锁定、以及 Markdown 列表小圆点强制高保真渲染)、136px 绝对统一卡片高度与纯标题模糊搜索、右侧抽屉安全管理锁与置顶/移至依次瞬移顺延排序体系、生成整页可视化 PRD 文档大屏 (带大纲检索与独立新网页大屏/PDF导出)、大头针气泡独立生命周期与展开侧边栏联动、编辑器草稿暂存最小化胶囊 (右下角悬浮保活)、以及多版本严格物理隔离与上传冲突解决模态框 (覆盖/追加/另存)。当用户提到「需求打点」「原型标注」「PRD标记」「元素打标」「交互规约生成」「给原型加上PRD功能」「大头针标注」时使用本 Skill。
agent_created: true
---

# 交互式原型 PRD 打点标注与多版本规约生成系统 (PM Prototype PRD Pin Engine V6)

## 📖 概述

`pm-proto-prd-pin` 是一套**零外部重度依赖、即插即用**的高保真原型交互规约标注引擎（纯原生 Vanilla JS + CSS + Node/Python 本地微服务）。
它能为任何已有的静态 HTML 原型（后台管理系统、电商前台、移动端 H5 等）瞬间赋予专业的产品经理（PM）交互式打点标注、多版本管理、**交互式可视化表格直编**、纯净无边框逐行即时可视化文档工作台、纯白 Mermaid 流程图动态防截断渲染、全景文档大屏展示与真实磁盘持久化能力。

---

## 🎯 核心规范与模块功能模板 (1:1 完整实现标准)

### Phase 0: 🌐 原生全球多语言架构体系 (Native Multi-Language i18n Protocol)
- **四国语言内建覆盖**：
  - 🇨🇳 **简体中文 (`zh-CN`)**
  - 🇺🇸 **English (`en`)**
  - 🇯🇵 **日本語 (`ja`)**
  - 🇰🇷 **한국어 (`ko`)**
- **全要素动态本地化 (115 个词条字典全覆盖)**：
  - 抽屉左侧控制按钮、顶部操作栏、排序管理模式提示条与操作按钮、搜索占位符、需求卡片脚标、打点编辑器全量工具栏、可视化表格操作按钮、流程图渲染栏、全景 PRD 大屏所有元数据与目录大纲、版本冲突处理对话框等 **100% 字典覆盖**；
- **运行时无感知热切换与本地记忆**：
  - 抽屉顶部提供多语言切换下拉菜单，切换后即刻热重绘所有组件与徽标，并将偏好持久化至 `localStorage('prd_ui_lang')`；
- **强制前置语言确认规范 (Mandatory Phase 0 Inception)**：
  - **在使用本 Skill 执行构建时，Agent 必须首先向用户确认期望使用的语言框架（中文 / 英文 / 日文 / 韩文）**；
  - 最终生成出来的原型框架、`window.INITIAL_PRD_DATA` 初始需求规约数据、业务规则模板、以及表头字典，必须严格采用用户指定的对应语言！

---

### Module 1: 💾 双层持久化机制与标准后端接口协议 (Dual Persistence API Protocol)
- **标准数据模型 (Data Schema)**：
  ```typescript
  interface PinItem {
    id: number;              // 1..N 连续自然序号
    title: string;           // 需求标题
    type: '业务规则' | '交互逻辑' | '数据口径' | '权限规则' | '异常流' | 'UI规范';
    desc: string;            // Markdown 格式内容 (支持表格/Mermaid/列表)
    selector: string;        // 自愈弹性 CSS 选择器
    rect: { top: number; left: number; width: number; height: number };
    pageKey: string;         // 如 "merchant.html"
    pageTitle: string;       // 如 "商家端后台"
    version: string;         // 如 "v1.0.0"
    updatedAt: string;       // ISO 时间戳
  }

  interface VersionRegistry {
    activeVersion: string;
    versions: {
      [versionName: string]: PinItem[];
    };
  }
  ```
- **REST API 标准接口规范 (优先调用)**：
  - `POST /api/save-prd`：
    - **请求体 (Payload)**：`{ pageKey: string, version: string, data: PinItem[], versionRegistry: VersionRegistry }`
    - **返回体 (Response)**：`{ success: true, message: "PRD data saved successfully" }`
    - **保存成功反馈**：右下角弹出 Toast `✅ 需求规约已成功保存并写入本地 JS 文件！`；
    - **保存失败反馈**：若未检测到本地服务接口（404/NetworkError），右下角弹出 Toast `❌ 保存失败：未检测到本地服务接口，无法写入本地磁盘 JS 文件！` 并提供一键下载备份；
  - `GET /api/get-all-prd`：
    - **返回体**：`{ success: true, registry: VersionRegistry }`
  - `GET /api/get-prd?page=merchant.html`：
    - **返回体**：`{ success: true, pins: PinItem[] }`
- **本地静态文件持久化 (Fallback)**：
  - 当无 Node.js 后端服务时，自动降级为 `localStorage` 缓存与点击「💾 导出 JS 数据」一键生成标准 `prd-data-[pageKey].js` 磁盘文件。

---

### Module 2: 📍 交互式十字准星打点与智能空间锚定 (Pinning & Spatial Anchoring Engine)
- **十字准星拾取器 (`bindPickListeners`)**：
  - 点击「📍 新增打点」进入打标模式，鼠标指针变为 `crosshair`，底层元素高亮蓝色虚线框；
  - **打标时自动收起右侧抽屉**：进入十字准星打标模式时，右侧抽屉**自动完全收起**，折叠为边缘胶囊 `📍 点击页面组件打标 (ESC退出)`，彻底杜绝侧边栏遮挡底层页面组件；
  - 拾取完成或按 ESC 后，自动无缝呼出需求规格编辑工作台；
- **自愈式弹性 CSS 选择器算法 (`getElementSelector`)**：
  - 优先提取精准 `#id`、语义化类名、结构层级路径（`div:nth-of-type(n)`）与表单属性，保证动态重绘后打点依然 100% 准确对齐；
- **连续帧率发光框追踪算法 (Continuous Frame-Rate Glow Box Tracking)**：
  - 采用 `requestAnimationFrame` 在平滑滚动的 50 帧（约 800ms）全周期内实时调用 `getBoundingClientRect()` 动态重绘红色发光脉冲框（`position: fixed`），彻底解决页面平滑滚动导致的坐标漂移；
- **智能容器自动展开 (Smart Container Auto-Unfolding)**：
  - 当打点目标位于未激活的 Tab 面板、已关闭的弹窗或折叠面板中时，定位时自动触发父容器的激活事件（如 `click` / Tab 切换），确保目标元素完全可见后再平滑聚焦。

---

### Module 3: ✍️ 纯净无边框逐行即时可视化工作台 (Borderless In-situ Live Editor Workbench)
- **分块组件架构 (`splitMarkdownIntoBlocks` & `renderLiveBlocksUI`)**：
  - 内部将 Markdown 自动解析为段落、标题、表格、流程图、代码块等多类型独立块结构；
  - 非编辑行呈现为纯净无框高保真排版，点击任意行即时就地展开为无框源码输入；
- **精准光标落点算法 (`document.caretRangeFromPoint`)**：
  - 鼠标在非编辑行任意文字位置点击时，系统毫秒级计算点击字符偏移量，光标**直接精准落到用户点击的文字位置**，告别跳到行首的繁琐；
- **Markdown 列表小圆点强制高保真渲染 (List Bullet Guard)**：
  - 注入 `!important` 列表样式规则，彻底免疫 host 页面 CSS Reset 或 Tailwind 的 `list-style: none` 干扰；
  - `- 文本` 严格渲染为高保真无序列表圆点 `•`，`1. 文本` 严格渲染为有序列表序号；
- **工具条操作无缝焦点锁定 (Focus-Preserving Toolbar)**：
  - 点击加粗 `B`、斜体 `I`、删除线 `S`、标题 `H3`/`H4`、列表、待办、引用、代码块等任意格式按钮或插入表格/模板时，**焦点持续锁定在当前编辑的那一行**，绝不跳转到文档最底部；
- **真正交互式可视化表格直编 (Visual Table Editor)**：
  - 彻底告别 Markdown 管道符（`| col |`），插入后直接渲染为所见即所得的真实 HTML 表格；
  - 所有表头 `<th>` 与单元格 `<td>` 支持 `contenteditable="true"`，点击直接打字；
  - `Shift + Enter` 单元格内换行（自动转为 `<br>`）；`Enter` 跳下一行单元格；`Tab` 跳右侧下一格；
  - 表格操作栏：`➕ 加一行`、`➕ 加一列`、`➖ 删末行`、`➖ 删末列`、`🗑️ 删表格`；
  - 双向自动序列化同步为标准 Markdown 表格；
- **纯白底色 Mermaid 矢量图表引擎与动态防截断渲染**：
  - 纯白优雅卡片底色，图表渲染完成后自动分析 SVG `viewBox`，动态追加 `+28px` 底部安全补偿高度，杜绝复杂图表底部文字截断；
  - 支持在图表右上角点击 `✏️ 编辑流程图` 就地修改代码并一键渲染；
- **业务规约预置模板库 (Templates Dropdown)**：
  1. **数据字典/字段规约模板**：字段名、类型、必填、说明、枚举值；
  2. **复杂计算/口径规则模板**：指标名称、计算公式、统计维度、更新频率；
  3. **外部接口/契约规范模板**：接口路径、请求方式、入参、出参、错误码；
  4. **权限与角色流转矩阵模板**：角色名称、查看权限、操作权限、审批权限；
- **草稿暂存最小化胶囊 (`.prd-editor-mini-dock`)**：
  - 点击「➖ 最小化/看页面」，编辑器折叠至右下角悬浮胶囊，随意查阅原型并一键无损恢复草稿。

---

### Module 4: 🗂️ 三态抽屉与左边缘双按钮控制体系 (3-State Drawer & Dual Inward Handles)
- **左边缘双按钮控制把手组 (朝内指向抽屉，颜色与功能清晰区隔)**：
  - **上方按钮 (全收起)**：深色科技质感（`#0f172a`）+ 朝内向右箭头 `›`，点击完全收起抽屉；
  - **下方按钮 (半收起)**：专业亮蓝质感（`#2563eb`）+ 朝内紧凑标号条图标 `⇥`，点击收窄为 56px 纯数字标号竖条；
  - **收起状态极简呈现**：当抽屉完全收起时，页面右侧中间无任何多余漂浮物，仅保留右上角 `📌 需求打点 (n)` 胶囊把手；
- **三态无缝切换 (`全展开 400px` ⇄ `半收起标号条 56px` ⇄ `完全收起 0px`)**：
  - **全展开 (400px)**：展示完整需求卡片列表、多版本下拉框、完全模糊搜索栏；
  - **半收起紧凑标号竖条 (56px Mini Rail)**：抽屉收窄为右侧极窄的纯标号竖条 `[①][②][③][④]...`，完全释放底层 95% 页面画布；
    - 点击标号条中任意序号：页面平滑滚动并闪烁定位发光框，且**在目标组件旁精准弹出打点详情气泡 (`Inspect Popover`)**，无缝查看需求；
    - 鼠标悬停标号展示需求标题 Tooltip；
  - **完全收起**：折叠至右侧边缘胶囊 `📌 需求打点 (n)`；
- **100% 绝对统一卡片高度 (Strict 136px Unified Height)**：
  - 标题单行截断带省略号（`text-overflow: ellipsis`）；
  - 简介使用纯文本摘要提取算法（过滤代码块/表格/标记），锁定标准 2 行（36px）展示，杜绝高度塌陷；
  - 所有卡片高度**严格统一为 136px**，列表排版整齐如一；
- **纯标题完全模糊搜索与检索系统 (Fuzzy Title Search)**：
  - 支持中英文子串包含、空格多关键词分词（如 `订单 履约`）、以及字符流顺序模糊检索，配有一键清空按钮 `✕`。

---

### Module 5: 🔒 排序管理安全锁与依次顺延瞬移体系 (Sequential Cascade Reordering)
- **日常安全浏览模式（默认）**：
  - 卡片隐藏排序/删除按钮，禁用拖拽，彻底防止日常查阅和定位误触；
- **排序与删除管理模式（点击 `⚙️ 排序管理` 开启）**：
  - 抽屉浮现提示条与 `✓ 完成退出` 按钮；
  - **智能连续排序与依次瞬移顺延**：
    - `🔝 置顶`：直接将目标项瞬移至第 1 项，后续所有项目**自动依次向下顺延 (1 -> 2 -> 3...)**；
    - `🔢 移至`：输入目标序号，目标项精准插入，其余项目依次顺延；
    - `▲` / `▼`：单步上移/下移；
    - `⠿`：鼠标按住拖拽排序；
  - **列表滚动高度锁定与平滑高亮**：调整排序后列表滚动位置毫秒级锁定不跳动，瞬移到位的新卡片带有呼吸发光边框，页面大头针数字同步更新。

---

### Module 6: 🏷️ 严格多版本物理隔离与导入冲突控制 (Multi-Version & Import Resolver)
- **版本数据模型与向后兼容**：
  - 统一由 `versionRegistry` 全局维护：
    ```javascript
    window.PRD_VERSION_REGISTRY = {
      "activeVersion": "v1.0.0",
      "versions": {
        "v1.0.0": [ /* pins */ ],
        "v1.1.0": [ /* pins */ ]
      }
    };
    ```
- **严格版本物理隔离 (Strict Version Isolation & Zero Cross-Contamination)**：
  - 页面大头针徽标、右侧抽屉列表、打点详情气泡、以及全文 PRD 文档大屏**仅展示当前所选激活版本的数据**，互相版本的打点**完全物理隔离、互不可见**；
  - **新建版本一律为全新空白版本 (Default Clean Blank Slate)**：
    - 点击「➕」或「➕ 新建空白版本...」时，直接创建一个全新的空打点版本（打点数：0），方便 PM 从零开始标注下一迭代；
    - 若需要派生副本，可选择「📋 复制当前版本副本」；
- **上传 JS 文件与同版本智能冲突处理 (Conflict Modal)**：
  - 用户上传 `.js` / `.json` 文件时弹出冲突模态框：
    - 🔴 **【覆盖现有版本】**：清空当前版本旧打点，完全替换为上传内容；
    - 🟢 **【追加合并】**：保留旧打点，将上传打点追加至末尾并重新递增编号；
    - 🔵 **【另存为新版本】**：自动命名为 `[版本]_imported`。

---

### Module 7: 📑 全景 PRD 文档大屏与交付导出 (PRD Document Screen & Export)
- **抽屉常驻入口**：右侧抽屉底部常驻 `📑 查看完整PRD` 按钮；
- **独立新网页打开 (`↗️ 在新网页打开`)**：
  - 在独立新标签页中打开纯净 PRD 大屏文档，带固定 TOC 目录大纲；
  - 适配打印排版，按 `Ctrl+P / Cmd+P` 一键导出交付级 PDF；
- **多格式一键导出**：支持 `📥 导出 Markdown`、`💾 导出 JS 数据`、`🖨️ 打印 / 导出 PDF`。

---

## 🛠️ 项目工程结构与文件规范

```text
my-prototype/
├── admin.html               # 原型页面 A
├── mall.html                # 原型页面 B
├── merchant.html            # 原型页面 C
├── h5.html                  # 原型页面 D
├── merchant-h5.html         # 原型页面 E
├── server.js                # 本地持久化后端服务 (Node.js 原生零依赖，提供 /api/save-prd)
├── start.sh                 # 一键启动脚本 (自动拉起静态服务与持久化接口)
└── assets/
    └── js/
        ├── prd-pin-tool.js  # 核心引擎 (V6 可视化表格/纯净无框工作台/多版本/多语言)
        ├── prd-data-admin.js# admin.html 专属数据文件
        ├── prd-data-mall.js # mall.html 专属数据文件
        ├── prd-data-merchant.js
        ├── prd-data-h5.js
        └── prd-data-merchant-h5.js
```
