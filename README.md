# 📌 PM Prototype PRD Pinning & Multi-Version Spec Engine (V6)

> **The Professional Interactive PRD Pinning, Vditor IR Markdown Editor & Multi-Version Specification Engine for HTML Prototypes.**  
> Supports **Native 4-Language i18n (`en`, `zh-CN`, `ja`, `ko`)**, **Multi-level Tab / Shift+Tab List Indentation**, **Vditor IR WYSIWYG Editor**, **Dual Persistence REST API (`/api/save-prd`)**, **3-State Drawer with Dual Inward Handles**, and **Continuous Frame-Rate Glow Tracking**.

---

## 🌟 Key Capabilities & Features

| Capability | Technical Mechanism | PM Experience Benefit |
| :--- | :--- | :--- |
| **✍️ Vditor IR Markdown Editor** | Integrated Vditor (Instant Rendering mode) with local vendor + CDN auto-fallback | Typora-like document writing experience with zero lag |
| **⌨️ Multi-Level Tab Indentation** | Native `Tab` / `Shift+Tab` handling for nested lists (`-`, `1.`, `- [ ]`) | Natural nesting and multi-level list organization |
| **📊 Interactive Visual Table Editor** | 2D editable HTML matrix, direct cell typing, `Enter/Tab` navigation, 1-click add/del rows/cols | Completely eliminates markdown pipe `\| col \|` syntax friction; feels like Excel/Word |
| **🔄 Pure-White Mermaid Vector Engine** | White card container + dynamic SVG `viewBox` bottom padding (+28px) | Instant rendering of Statecharts, Sequence Diagrams, and Flowcharts without clipped labels |
| **💡 3 Core Business Templates** | Quick-insert buttons for Business Rules, Statechart Diagrams, and Data Dictionary Tables | High-speed authoring of standardized PM specification clauses |
| **👀 Single-Entry Stash & Minimize** | Header clean close `✕` + Bottom bar `👀 Stash & View Page` (`.prd-editor-mini-dock` pill) | Effortlessly review the underlying prototype without losing active draft |
| **🌐 Native 4-Language i18n** | Full 115-key dynamic dictionary for `en`, `zh-CN`, `ja`, `ko`; runtime switching with `localStorage` memory | Seamless multi-lingual team collaboration across English, Chinese, Japanese, and Korean |
| **💾 Dual Persistence REST API** | Prioritizes `POST /api/save-prd` payload sync; graceful fallback to `prd-data.js` and `localStorage` | Zero data loss, true disk persistence without complex database setup |
| **🗂️ 3-State Drawer & Dual Handles** | Full (400px), Semi (56px Mini Rail), Hidden (0px); dual inward edge handles (Top: Full Close `›`, Bottom: Semi `⇥`) | Uncovers 95% of prototype canvas while keeping all pins immediately accessible |
| **🔒 Reorder Safety Lock & Cascade** | Normal browsing locks order; manage mode unlocks `🔝 Top`, `🔢 Move To`, `▲/▼`, sequential downward cascade | Accidental reordering prevention; seamless 1 -> 2 -> 3 cascading when moving items |
| **🏷️ Multi-Version Physical Isolation** | Isolated `versionRegistry`; new versions start clean (pins: 0); upload conflict resolver | No pin cross-contamination across sprint iterations |
| **📑 Full PRD Document & PDF Export** | Dedicated doc modal + new tab view with TOC outline and print styling | 1-click export to deliverable PDF or Markdown spec |

---

## 📸 Real Interface Screenshots

### 1. Vditor IR Live Editor (Mermaid & Data Dictionary Table)
![Vditor Live Editor](assets/images/screenshot_visual_table_editor.png)

### 2. Minimized Stash Pill (Bottom-Right Floating Dock)
![Minimized Stash Dock](assets/images/screenshot_drawer_overview.png)

### 3. Pure White Mermaid Flowchart Engine
![Mermaid Render](assets/images/screenshot_mermaid_render.png)

### 4. Full Page PRD Specification Document
![Full PRD Document](assets/images/screenshot_full_prd_document.png)

---

## 🚀 Quick Start Guide

### 1. Install via Antigravity / Claude Code
```bash
# Verify skill installation
ls ~/.gemini/config/skills/pm-proto-prd-pin/
```

### 2. Embed into HTML Prototype
Add the following snippet before the closing `</body>` tag of your prototype:

```html
<!-- PRD Data File (Auto-generated per page) -->
<script src="assets/js/prd-data-merchant.js"></script>

<!-- PRD Pinning & Visual Editor Engine -->
<script src="assets/js/prd-pin-tool.js"></script>
```

### 3. Start Local Persistence Server
```bash
# Start Node.js native zero-dependency server
node server.js
```
Access `http://localhost:3000/merchant.html` and start pinning specs!

---

## 🇨🇳 中文说明与规范

### 🌟 核心特性概览

1. **✍️ Vditor IR 即时渲染 Markdown 工作台**：
   - 类似 Typora 的流式直编体验，输入即渲染；
   - 优先加载本地 `assets/vendor/vditor/` 离线文件，离线/缺失时自动降级为 CDN。
2. **⌨️ 完整支持 Tab / Shift+Tab 多级层级缩进**：
   - 在 `- 列表` 或 `1. 序号` 下敲击 `Tab` 即刻向右缩进多级嵌套；
   - 敲击 `Shift + Tab` 或行首 `Backspace` 瞬间回退层级。
3. **📊 真正的交互式可视化表格直编**：
   - 零 Markdown 竖线管道符，所见即所得真实 HTML 表格；
   - 单元格支持直接打字、`Enter/Tab` 换行换格、一键增删行列。
4. **💡 三大核心业务规约模板一键插入**：
   - **「📋 业务规则模版」**、**「🔄 状态机流程图」**、**「📊 字段数据字典表」**。
5. **👀 单一入口草稿暂存与最小化胶囊**：
   - 弹窗右上角保留极简 `✕`，底部保留 **「👀 暂存并看页面」**；
   - 点击后折叠为右下角常驻胶囊（`✏️ 编辑中: 需求名称 · 草稿已暂存`），不遮挡原型，一键恢复。
6. **🌐 全球 4 语言架构 (`zh-CN`, `en`, `ja`, `ko`)**：
   - 抽屉顶部实时切换多语言，115 个字典键全要素动态本地化；
   - 规则强制 Phase 0 前置确认目标语言框架。
7. **💾 REST API 双层持久化标准**：
   - 优先调用 `POST /api/save-prd` 写入本地磁盘 JS 文件；
   - 本地服务离线时自动降级为 `localStorage` 缓存与一键导出 JS 脚本。
8. **🗂️ 三态抽屉与左边缘双按钮控制组**：
   - 400px 全展开 ⇄ 56px 紧凑标号竖条 ⇄ 0px 完全收起；
   - 左侧双按钮朝内区隔（上方深色全收起 `›` + 下方亮蓝半收起 `⇥`）。
9. **🔒 排序管理安全锁与依次瞬移顺延**：
   - 日常防误触锁定，开启管理模式后支持 `🔝 置顶`、`🔢 移至`、`▲/▼`；
   - 置顶或上移时后续项自动依次向下顺延（1 -> 2 -> 3...），滚动高度毫秒级锁定。
10. **🏷️ 多版本严格物理隔离与上传冲突解决**：
    - 新建版本默认全新空白（0 打点），各版本数据完全隔离互不干扰；
    - 上传文件支持【覆盖现有】、【追加合并】、【另存新版本】三种冲突方案。

---

## 📄 License
MIT License. Created for Product Managers & UI/UX Engineers.
