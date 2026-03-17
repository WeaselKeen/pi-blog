---
title: "Chrome CDP Skill 安装指南 - AI 接管浏览器"
description: "用 chrome-cdp-skill 让 AI 稳定控制浏览器"
pubDate: "2026-03-17"
heroImage: "../../assets/blog-placeholder-3.jpg"
---

# 🚀 Chrome CDP Skill 安装指南

> Chrome 146+ 支持 MCP，但官方包有 bug。推荐用 chrome-cdp-skill，更稳定。

---

## 一、开启 Chrome 远程调试

1. 更新 Chrome 到 146+
2. 打开 `chrome://inspect/#remote-debugging`
3. 勾选 **Allow remote debugging for this browser instance**
4. 记住显示的地址：`127.0.0.1:9222`

---

## 二、安装 chrome-cdp-skill

```bash
# Mac/Linux
npx skills add https://github.com/pasky/chrome-cdp-skill -g --all --copy

# Windows (有人适配的版本)
npx skills add https://github.com/hanyu0001/chrome-cdp-skill -g --all --copy
```

---

## 三、连接 AI Agent

安装后，在 Claude Code / Codex 等 AI 工具中导入 skill 即可使用。

---

## 四、优势对比

| 特性 | 官方 chrome-devtools-mcp | chrome-cdp-skill |
|------|-------------------------|-----------------|
| 弹窗 | 每次都弹 | 只需授权一次 |
| 响应速度 | 慢 | 快 |
| 稳定性 | 易超时 | 稳定 |
| 鼠标 | 会抢焦点 | 静默控制 |

---

## 五、实测效果

- AI 可以直接操作网页
- 继承已登录 Cookie
- 支持精准 DOM 点击
- 不会抢鼠标焦点

---

**参考来源：** @雨哥向前冲 (10K+ views)
