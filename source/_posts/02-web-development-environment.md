---
title: 实验二 Web 软件开发环境
date: 2026-07-10 16:00:00
categories:
  - 实验记录
tags:
  - Node.js
  - npm
  - Git
  - Hexo
---

实验二主要完成 Web 开发环境搭建，包括 Node.js、npm、Git 和 Hexo CLI 的安装与验证，并初始化一个 Hexo 静态博客项目。

## 环境验证

通过命令行检查 Node.js、npm、Git 和 Hexo CLI 的版本，确认基础开发环境可用。

![环境验证](/images/exp2/exp2_01_environment.png)

## 安装依赖

进入 Hexo 项目目录后执行 `npm install`，安装项目依赖。安装过程中出现少量 deprecated 警告，但最终依赖安装成功。

![npm install](/images/exp2/exp2_05_npm_install_success.png)

## 生成静态文件

执行 `hexo generate`，Hexo 成功生成首页、归档页、CSS 和 JavaScript 文件。

![hexo generate](/images/exp2/exp2_06_hexo_generate_success.png)

## 启动本地服务

执行 `hexo server`，本地服务运行在 `http://localhost:4000/`。

![hexo server](/images/exp2/exp2_07_hexo_server_running.png)

## 浏览器访问

在浏览器中访问本地地址，成功显示 Hexo 默认首页。

![Hexo 首页](/images/exp2/exp2_08_hexo_homepage.png)

## 小结

实验二完成了 Web 开发环境搭建，为后续站点构建和部署提供了基础。

