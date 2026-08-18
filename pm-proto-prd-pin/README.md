# 📌 pm-proto-prd-pin (交互式原型 PRD 打点与多版本规约生成系统)

> **为任意 HTML 原型一键植入「零依赖、即插即用」的专业级产品经理 (PM) 交互式打点标注与多版本规约生成工作台。**

---

## 🌟 核心特性概览

| 特性模块 | 核心能力 | 解决痛点 |
|---|---|---|
| **📊 可视化表格直编** | 支持直接在单元格打字、`Shift+Enter` 内换行、`Enter/Tab` 智能跳格增行，自动双向序列化 Markdown | 告别繁琐的 `| col |` 竖线语法，像 Excel 一样自然直观 |
| **✍️ 逐行即时可视化** | Obsidian / Typora 级行内所见即所得：当前行源码输入，非编辑行全自动实时富文本渲染 | 无需来回点击切换预览，所见即所得，文档流畅手感 |
| **🏷️ 严格多版本隔离** | 物理级版本隔离（互相版本打点绝对互不可见）、新建默认为全新空白版本、多策略导入冲突处理 | 满足多迭代需求规划，避免不同版本需求混淆污染 |
| **🔄 纯白防截断图表** | Mermaid 矢量流程图纯白优雅容器，根据 SVG viewBox 动态补足 +28px 底部安全高度 | 消除暗色代码块杂乱感，复杂多层流程图永不截断 |
| **🔍 标题完全模糊搜索** | 拼音/字符流/分词模糊检索，抽屉卡片与页面大头针徽标毫秒级联动响应 | 需求过多时一秒定位目标规约 |
| **🔒 安全管理锁模式** | 默认安全浏览防误触；开启管理模式后方可拖拽排序、`🔢 移至` 输入序号或删除 | 杜绝日常查看和定位时的误触 |
| **🎯 帧率追踪精准定位** | `position: fixed` 绝对坐标系统 + 800ms 动态帧率追踪发光框 | 彻底消除页面滚动导致的位移漂移偏差 |
| **📑 全景 PRD 大屏 & PDF** | 内置固定 TOC 大纲导航，支持 `↗️ 在新网页打开` 与按 `Ctrl+P / Cmd+P` 一键导出交付级 PDF | 随时随地生成交付级专业 PRD 说明文档 |

---

## 📸 真实运行效果截图

### 1. 📋 页面大头针标注与右侧需求抽屉 (Overview)
页面任意元素均可直观可视打点，右侧抽屉常驻展示需求列表、版本切换与模糊搜索栏。

![右侧需求抽屉与页面大头针标注](assets/images/screenshot_drawer_overview.png)

---

### 2. 📊 交互式可视化表格直编与纯净无框工作台 (Visual Table Editor)
表格直接呈现为排版表格，点击单元格直接打字输入，支持一键增删行列与 `Shift+Enter` 单元格内换行。

![交互式可视化表格直编与纯净无框工作台](assets/images/screenshot_visual_table_editor.png)

---

### 3. 🔄 纯白底色 Mermaid 矢量流程图与动态防截断引擎 (Mermaid Flowchart)
全局消除暗色背景干扰，纯白独立优雅卡片呈现，复杂流程图底部文字与节点 100% 完整显示。

![纯白底色 Mermaid 矢量流程图与防截断渲染](assets/images/screenshot_mermaid_render.png)

---

### 4. 📑 全景 PRD 需求规格大屏与目录检索 (Full Document View)
一键呼出全屏 PRD 规约大屏，左侧目录树快速定位，支持新网页独立打开与一键打印导出 PDF。

![全景 PRD 需求规格大屏与目录检索](assets/images/screenshot_full_prd_document.png)

---

## 🛠️ 项目目录结构

```text
my-prototype-project/
├── admin.html               # 运营端后台原型
├── mall.html                # 商城端前台原型
├── merchant.html            # 商家端后台原型
├── h5.html                  # 买家 H5 移动端原型
├── server.js                # 本地持久化后端服务 (Node.js 原生零依赖)
├── start.sh                 # 一键启动脚本
└── assets/
    └── js/
        ├── prd-pin-tool.js  # 核心引擎 (V6 逐行可视化/表格直编/版本隔离)
        ├── prd-data-admin.js# admin.html 专属数据文件
        ├── prd-data-mall.js # mall.html 专属数据文件
        └── prd-data-merchant.js
```

---

## 🚀 极速上手使用指南

### 第一步：引入脚本

在任意 HTML 原型文件的 `</body>` 结束标签前引入当前页面对应的数据文件与核心打点引擎：

```html
  <!-- 引入当前页面专属 PRD 数据与打点引擎 -->
  <script src="assets/js/prd-data-mall.js"></script>
  <script src="assets/js/prd-pin-tool.js"></script>
</body>
</html>
```

---

### 第二步：配置轻量持久化服务端 (`server.js`)

Node.js 原生零依赖后端，自动支持 `/api/save-prd` 接口落盘与多版本注册表维护：

```javascript
const http = require('http');
const fs = require('fs');
const path = require('path');

const PORT = process.env.PORT || 3000;
let BASE_DIR = __dirname;
if (fs.existsSync(path.join(__dirname, 'platform'))) {
  BASE_DIR = path.join(__dirname, 'platform');
}

const MIME_TYPES = {
  '.html': 'text/html; charset=utf-8',
  '.css': 'text/css; charset=utf-8',
  '.js': 'application/javascript; charset=utf-8',
  '.json': 'application/json; charset=utf-8',
  '.png': 'image/png',
  '.jpg': 'image/jpeg',
  '.svg': 'image/svg+xml'
};

const server = http.createServer((req, res) => {
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

  if (req.method === 'OPTIONS') {
    res.writeHead(204);
    res.end();
    return;
  }

  const parsedUrl = new URL(req.url, `http://${req.headers.host}`);
  const pathname = decodeURIComponent(parsedUrl.pathname);

  // 1. 多版本 PRD 数据落盘写入
  if (pathname === '/api/save-prd' && req.method === 'POST') {
    let body = '';
    req.on('data', chunk => body += chunk);
    req.on('end', () => {
      try {
        const payload = JSON.parse(body);
        const { page, data, versionRegistry } = payload;
        const cleanPageName = page.replace('.html', '').replace(/^prd-data-/, '');
        const jsDir = path.join(BASE_DIR, 'assets', 'js');
        const pageFilePath = path.join(jsDir, `prd-data-${cleanPageName}.js`);

        const fileContent = `/**\n * PRD 需求数据 - ${cleanPageName}\n * 本地实时保存于: ${new Date().toLocaleString()}\n */\nwindow.INITIAL_PRD_DATA = ${JSON.stringify(data || [], null, 2)};\n${versionRegistry ? `window.PRD_VERSION_REGISTRY = ${JSON.stringify(versionRegistry, null, 2)};\n` : ''}`;

        fs.mkdirSync(jsDir, { recursive: true });
        fs.writeFileSync(pageFilePath, fileContent, 'utf-8');

        res.writeHead(200, { 'Content-Type': 'application/json; charset=utf-8' });
        res.end(JSON.stringify({ success: true, message: `成功写入 ${pageFilePath}` }));
      } catch (err) {
        res.writeHead(500, { 'Content-Type': 'application/json; charset=utf-8' });
        res.end(JSON.stringify({ success: false, error: err.message }));
      }
    });
    return;
  }

  // 2. 静态页面托管
  let filePath = path.join(BASE_DIR, pathname);
  if (filePath.endsWith('/') || filePath === BASE_DIR) {
    filePath = path.join(BASE_DIR, 'mall.html');
  }

  const ext = path.extname(filePath).toLowerCase();
  const contentType = MIME_TYPES[ext] || 'application/octet-stream';

  fs.readFile(filePath, (err, content) => {
    if (err) {
      res.writeHead(404, { 'Content-Type': 'text/plain; charset=utf-8' });
      res.end('404 Not Found');
    } else {
      res.writeHead(200, { 'Content-Type': contentType });
      res.end(content);
    }
  });
});

server.listen(PORT, () => {
  console.log(`🚀 原型打点服务已启动: http://localhost:${PORT}`);
});
```

---

## ⌨️ 常用快捷键与操作指南

- **新增打点**：点击抽屉底部 `📍 新增打点`，鼠标变为十字准星，直接点击原型界面任意元素即可完成绑定并呼出编辑窗；
- **表格快捷键**：
  - `Shift + Enter`：在单元格内直接换行（自动转为 Markdown `<br>`）；
  - `Enter`：跳转至下一行同列单元格（末行自动新增一行）；
  - `Tab`：跳转至右侧下一单元格（末尾自动新增一行）；
  - `Shift + Tab`：返回上一单元格；
- **最小化草稿**：点击编辑窗右上角 `➖ 最小化/看页面`，折叠至右下角悬浮胶囊，随意查阅原型界面并一键无损还原；
- **导出 PDF**：点击 `📑 查看完整PRD` → `↗️ 在新网页打开` → 在浏览器按 `Ctrl+P / Cmd+P` 直接打印导出。
