---
title: "WebMCP 完全上手指南 - 2026 版"
description: "Google Chrome 官方发布的 AI Agent 网页交互协议"
pubDate: "2026-03-16"
heroImage: "/blog-placeholder-2.jpg"
---

# 🚀 WebMCP 完全上手指南 - 2026 版

> 2026年3月，Google Chrome 官方发布 WebMCP，可能是 AI Agent 领域最重要的里程碑。

---

## 什么是 WebMCP？

**WebMCP (Web Model Context Protocol)** 是 Google 推出的网页版 MCP（Model Context Protocol）实现。

### 对比传统方式

| 方式 | 原理 | 缺点 |
|------|------|------|
| **屏幕截图** | AI 看图理解 | 信息丢失、慢、不准确 |
| **WebMCP** | AI 直接调用网页工具 | 快、准、可交互 |

### 举个例子

**以前**：让 AI 帮你订外卖
- AI 截图 → 分析图片 → 理解布局 → 点击按钮
- 慢！且容易点错

**现在**：让 AI 帮你订外卖
- AI 直接调用网页的 `addToCart()`、`checkout()` 工具
- 快！和真人操作一样

---

## 一、环境准备

### 1.1 安装 Chrome Canary

WebMCP 目前**只有 Chrome Canary** 支持。

```bash
# macOS
brew install --cask google-chrome-canary

# Windows
# 下载: https://www.google.com/chrome/canary/

# Linux
sudo apt install google-chrome-canary
```

### 1.2 开启 WebMCP Flag

1. 打开 Chrome Canary
2. 地址栏输入：`chrome://flags/#enable-webmcp-testing`
3. 找到 **Enable WebMCP** 选项
4. 改为 **Enabled**
5. 点击 **Relaunch** 重启浏览器

### 1.3 验证安装

打开任意 HTTPS 网站，按 F12 打开开发者工具，在 Console 输入：

```javascript
navigator.modelContext
```

如果返回 `true`，说明 WebMCP 已开启成功！

---

## 二、基础概念

### 2.1 工具 (Tools)

网页开发者暴露给 AI 调用的函数。

```javascript
// 网页端注册一个工具
navigator.modelContext.registerTool({
  name: "add_to_cart",
  description: "将商品添加到购物车",
  inputSchema: {
    type: "object",
    properties: {
      productId: { type: "string", description: "商品ID" },
      quantity: { type: "number", description: "数量" }
    },
    required: ["productId"]
  },
  execute: async (params) => {
    // 调用后端 API
    const response = await fetch('/api/cart', {
      method: 'POST',
      body: JSON.stringify(params)
    });
    return await response.json();
  }
});
```

### 2.2 提示 (Prompts)

预定义的提示模板。

```javascript
navigator.modelContext.registerPrompt({
  name: "analyze_product",
  description: "分析当前商品页面的信息",
  template: "请分析这个商品的：名称、价格、规格、用户评价"
});
```

### 2.3 资源 (Resources)

AI 可以读取的网页数据。

```javascript
navigator.modelContext.registerResource({
  uri: "cart://current",
  description: "当前购物车内容",
  mimeType: "application/json",
  async: true,  // 动态获取
  fetch: async () => {
    const res = await fetch('/api/cart');
    return await res.json();
  }
});
```

---

## 三、快速实战

### 3.1 做一个"AI 助手购物"功能

假设你有一个电商网站，添加 WebMCP 后：

```html
<script>
// 产品页面
navigator.modelContext.registerTool({
  name: "addToCart",
  description: "将商品添加到购物车",
  inputSchema: {
    type: "object",
    properties: {
      sku: { type: "string" },
      qty: { type: "number", default: 1 }
    }
  },
  execute: async ({ sku, qty }) => {
    const res = await fetch('/api/cart/add', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ sku, qty })
    });
    return { success: true, cartSize: res.cartSize };
  }
});

navigator.modelContext.registerTool({
  name: "getProductInfo",
  description: "获取当前商品信息",
  execute: async () => {
    return {
      name: document.querySelector('.product-title').textContent,
      price: document.querySelector('.price').textContent,
      sku: document.querySelector('[data-sku]').dataset.sku
    };
  }
});
</script>
```

### 3.2 AI 怎么调用？

用户对 AI 说："帮我买这个鼠标"

AI 收到后：
1. 调用 `getProductInfo()` → 获取产品信息
2. 调用 `addToCart({ sku: "mouse-001", qty: 1 })` → 加入购物车
3. 调用 `checkout()` → 结账

**全程自动化，就像有个助手在操作！**

---

## 四、MCP vs WebMCP

| 特性 | MCP (传统) | WebMCP |
|------|-----------|--------|
| 运行环境 | 本地/服务器 | 浏览器 |
| 适用场景 | API、数据库 | 网页应用 |
| 部署方式 | 需要安装 | 网页自带 |
| 隐私 | 数据在服务器 | 数据在本地浏览器 |

**可以一起用！** WebMCP 负责浏览器端，MCP 负责服务端。

---

## 五、开发者如何接入？

### 5.1 前端接入（2分钟）

```html
<script src="https://cdn.jsdelivr.net/npm/@chrome/webmcp"></script>
<script>
  // 注册你的工具
  navigator.modelContext.registerTool({...});
</script>
```

### 5.2 后端接入

```javascript
// Node.js
import { WebMCPServer } from '@chrome/webmcp';

const server = new WebMCPServer({
  tools: {
    searchProducts: async (query) => {
      return await db.products.search(query);
    }
  }
});

server.listen(3000);
```

---

## 六、常见问题

### Q: 哪些浏览器支持？
A: 目前只有 **Chrome Canary** (2026年3月)

### Q: 安全吗？
A: 安全！工具调用需要用户授权，且只能在当前页面上下文中执行。

### Q: 和 Selenium/Playwright 比有什么区别？
A: 
- Selenium/Playwright：**程序员**写的自动化脚本
- WebMCP：**AI** 自己决定的自动化操作

### Q: 国内能用吗？
A: 可以！Chrome Canary 国内可以直接下载使用。

---

## 七、为什么这是 big deal？

1. **AI Agent 的最后一公里** - 之前 AI 只能"看"网页，现在能"操作"网页
2. **用户体验革命** - 就像有个真人帮你操作网站
3. **万亿市场** - 所有需要操作的场景都能用：购物、订票、填表、自动化办公...

---

## 八、下一步

1. **体验**：安装 Chrome Canary，开启 WebMCP
2. **开发**：给你的网站接入 WebMCP
3. **学习**：关注 Chrome Developers 官方账号

**2026年，会是 WebMCP 元年。**

---

*教程整理时间：2026年3月*
*来源：Chrome 官方博客 + 各开发者社区*
