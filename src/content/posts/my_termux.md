---
title: CF部署LibreTV详细教程
published: 2025-08-28
pinned: true
description: 利用 Cloudflare Pages 零成本、一键部署 LibreTV 在线影视搜索站，含自定义域名、密码保护及常见问题排查。
tags: [LibreTV, Cloudflare, 部署教程, 影视站]
category: 实战教程
draft: false
licenseName: "CC-BY-SA-4.0"
author: your_name
sourceLink: "https://github.com/LibreSpark/LibreTV"
---

# CF部署LibreTV详细教程

> 通过 Cloudflare Pages 免费、快速、稳定地部署 LibreTV，实现个人在线影视库。  
> 全平台支持（PC、手机、Pad），可设置密码、绑定自定义域名，国内访问速度优秀[^1^][^4^]。

---

## 🔍 LibreTV 简介

- **功能**：聚合搜索多个影视站点，支持在线播放、弹幕、历史记录  
- **技术栈**：纯前端（HTML+JS），无数据库，部署简单  
- **官方仓库**：[LibreSpark/LibreTV](https://github.com/LibreSpark/LibreTV)  
- **在线演示**：<https://libretv.pages.dev>（访问密码 `aizrf`）

---

## 🚀 部署总览

| 方案 | 优点 | 适用场景 |
|---|---|---|
| **Cloudflare Pages**（本文重点） | 免费、全球 CDN、HTTPS、自动部署 | 无服务器、零成本、外网访问[^10^] |
| Vercel | 同样免费，海外速度佳 | 海外用户 |
| Docker / NAS | 局域网高速、数据私有 | 本地或 NAS 用户 |

---

## 📦 前置条件

1. GitHub 账号（用于 Fork 仓库）  
2. Cloudflare 账号（托管服务，[注册](https://dash.cloudflare.com/sign-up)）  
3. 可选：一个自己的域名（用于自定义访问地址）

---

## 🚧 步骤一：Fork 官方仓库

1. 打开 [LibreTV 仓库](https://github.com/LibreSpark/LibreTV)  
2. 点击右上角 **Fork** → 选择你的账户 → 完成 Fork  
   （以后更新只需在自己仓库点击 **Sync fork** 即可）

---

## 🚧 步骤二：创建 Cloudflare Pages 项目

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)  
2. 侧边栏 **Pages** → **Create a project** → **Connect to Git**  
3. 选择刚刚 Fork 的 `LibreTV` 仓库 → **Begin setup**

---

## ⚙️ 步骤三：构建设置

| 字段 | 填写内容 |
|---|---|
| Project name | 任意，如 `my-libretv` |
| Framework preset | **None** |
| Build command | 留空 |
| Build output directory | 留空（根目录） |

点击 **Save and Deploy** 等待约 1~2 分钟，首次部署完成！

---

## 🔐 步骤四：开启密码保护（可选但强烈建议）

1. 部署完成后 → 进入 Pages **项目设置** → **Environment variables**  
2. 添加变量：

| 变量名 | 示例值 | 说明 |
|---|---|---|
| `PASSWORD` | `123456` | 用户访问密码 |
| `ADMINPASSWORD` | `admin888` | 管理后台密码（可选）[^6^] |

3. 保存后 Pages 会自动触发一次新部署，生效即可。

---

## 🌍 步骤五：自定义域名（可选）

1. 在 **Pages 项目设置** → **Custom domains** → **Set up a custom domain**  
2. 输入你的域名（如 `tv.example.com`），Cloudflare 将自动生成 CNAME 记录  
3. 到你的 DNS 服务商添加该 CNAME，指向 `<project>.pages.dev`  
4. 几分钟后 HTTPS 证书自动下发，即可通过自定义域名访问

---

## 🎯 访问与使用

- 默认地址：`https://<project>.pages.dev`  
- 首次打开输入设置的 `PASSWORD` 即可进入主页  
- **设置面板**：右上角齿轮图标  
  - 数据源：可全选或自定义 API 接口  
  - 播放器：切换解析线路、开启/关闭弹幕  
  - 主题：浅色/深色/自动

---

## 🛠️ 高级配置（可选）

配置文件位于仓库根目录 `js/config.js`，可在线编辑或本地修改后推送：

| 配置项 | 说明 | 示例 |
|---|---|---|
| `PROXY_URL` | 自建反代，解决部分资源跨域 | `https://cors.yourdomain.com/` |
| `API_SITES` | 增删影视源 API | 见官方 Wiki |
| `SITE_CONFIG` | 站点名称、Logo、备案号 | `{title:'我的影视站'}` |
| `HIDE_BUILTIN_ADULT_APIS` | 隐藏敏感源 | `true` / `false` |

> **注意**：修改后需前往 **Cloudflare Pages** → **Deployments** → **Retry deployment** 重新部署以生效。

---

## 🐛 常见问题 FAQ

| 问题 | 解决方案 |
|---|---|
| 打开空白/404 | 检查环境变量是否已重新部署；确认仓库同步最新 |
| 播放无画面 | 切换解析线路或检查浏览器广告拦截插件 |
| 搜索无结果 | 在设置中勾选更多数据源，或自建反代 |
| 访问缓慢 | 开启 Cloudflare **Always Online** 与 **Caching Level: Standard** |

---

## 📚 其他部署方式一键命令

- **Vercel**：Fork 后直接导入仓库，环境变量同上  
- **Docker**：

```bash
docker run -d \
  --name libretv \
  -p 8899:8080 \
  -e PASSWORD=123456 \
  --restart unless-stopped \
  bestzwei/libretv:latest
56