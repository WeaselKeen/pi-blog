---
title: "WebMCP 1分钟安装指南"
description: "Chrome AI Agent 协议一键开启"
pubDate: "2026-03-16"
heroImage: "../../assets/blog-placeholder-3.jpg"
---

# 🚀 WebMCP 1分钟安装指南

## 一、安装 Chrome Canary

```bash
# macOS
brew install --cask google-chrome@canary

# Windows
# 下载: https://www.google.com/chrome/canary/
```

## 二、开启 WebMCP

1. 打开 Chrome Canary
2. 复制粘贴这行到地址栏：
```
chrome://flags/#enable-webmcp-testing
```
3. 点击 **Enabled**
4. 点击 **Relaunch** 重启浏览器

## 三、验证安装

随便打开一个网页（如 https://example.com），按 F12 打开开发者工具，在 Console 输入：

```javascript
navigator.modelContext
```

如果返回 `true`，就成功了！🎉

## 四、体验 Demo

打开这个 Demo 页面体验 AI 操作网页：
https://webmcp-demo.example.com

（或者等官方出更多 Demo）

---

**最简总结：**
1. `brew install --cask google-chrome@canary`
2. 打开 `chrome://flags/#enable-webmcp-testing`，点 Enabled
3. 重启浏览器
4. 验证：`navigator.modelContext` 返回 `true`

就这么简单！
