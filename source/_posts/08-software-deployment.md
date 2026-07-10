---
title: 实验八 软件部署
date: 2026-07-10 19:00:00
categories:
  - 实验记录
tags:
  - Deploy
  - GitHub Actions
  - GitHub Pages
  - Nginx
  - rsync
---

实验八完成课程展示站点的自动化部署。项目使用 GitHub Actions 构建 Hexo 静态站点，并发布到 GitHub Pages。

## 部署信息

| 项目 | 内容 |
|---|---|
| 源码仓库 | https://github.com/cerry-han/software-tools-practice |
| 部署方式 | GitHub Actions + GitHub Pages |
| 发布分支 | gh-pages |
| 线上地址 | https://cerry-han.github.io/software-tools-practice/ |

## 自动化部署流程

项目推送到 `main` 分支后，GitHub Actions 自动执行：

1. 检出源码。
2. 安装 Node.js。
3. 安装 npm 依赖。
4. 执行 `npm run build` 生成静态站点。
5. 将 `public/` 目录发布到 `gh-pages` 分支。

核心配置文件：

```text
.github/workflows/deploy.yml
```

## 工作流配置

```yaml
name: Deploy Hexo site

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source
        uses: actions/checkout@v4
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: npm
      - name: Install dependencies
        run: npm install
      - name: Generate static site
        run: npm run build
      - name: Deploy to gh-pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          publish_branch: gh-pages
```

## 服务器部署

在 Ubuntu 虚拟机 node2 上安装 Nginx，并使用 SCP/rsync 同步 Hexo 的 `public/` 目录：

```bash
sudo apt install -y nginx
sudo systemctl enable --now nginx
npm run build
scp -r public/* dev@192.168.56.102:/var/www/blog/software-tools-practice/
```

虚拟机访问地址：

```text
http://192.168.56.102/software-tools-practice/
```

## 小结

实验八实现了课程站点从源码到线上访问的完整流程。自动化部署降低了手动上传文件的成本，也让每次修改网站内容后都可以通过 GitHub Actions 自动发布；Nginx 和 rsync 部署流程则补充了 Linux 服务器中的传统静态站点发布方式。
