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
  - Linux
---

实验八完成课程展示站点从源码构建到线上发布的完整流程。本实验包含两条部署路线：一条是 GitHub Actions 自动构建并发布到 GitHub Pages，另一条是将 Hexo 生成的静态文件部署到 Ubuntu Server 虚拟机中的 Nginx 服务。

## 一、实验目的

通过本实验掌握静态站点的构建、发布和访问验证流程。实验重点包括 GitHub Actions 自动化部署、GitHub Pages 在线托管、Ubuntu Server 中 Nginx Web 服务配置，以及通过 SSH/SCP 将静态站点发布到虚拟机服务器。

## 二、实验环境

| 项目 | 内容 |
|---|---|
| 源码仓库 | https://github.com/cerry-han/software-tools-practice |
| 静态站点框架 | Hexo |
| 自动部署 | GitHub Actions + GitHub Pages |
| 线上地址 | https://cerry-han.github.io/software-tools-practice/ |
| 虚拟机系统 | Ubuntu Server 22.04.5 LTS |
| 虚拟机主机名 | node2 |
| 虚拟机用户 | dev |
| Host-Only 地址 | 192.168.56.102 |
| Web 服务 | Nginx |
| 虚拟机访问地址 | http://192.168.56.102/software-tools-practice/ |

## 三、实验过程

### 步骤一：配置 GitHub Actions

在项目目录中创建 `.github/workflows/deploy.yml`，配置当 `main` 分支发生 push 时自动执行构建和发布。工作流主要步骤包括检出源码、安装 Node.js、安装 npm 依赖、执行 Hexo 构建，并把 `public/` 目录发布到 `gh-pages` 分支。

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

![GitHub Actions 自动部署成功](/images/exp8/exp8_01_actions_success.png)

*图1：GitHub Actions 自动部署任务执行成功*

### 步骤二：启用 GitHub Pages

进入 GitHub 仓库的 Settings -> Pages，将发布来源设置为 `gh-pages` 分支，目录选择 `/root`。保存后 GitHub Pages 会生成线上站点地址。

![GitHub Pages 站点已上线](/images/exp8/exp8_02_pages_live.png)

*图2：GitHub Pages 显示站点已上线*

### 步骤三：确认虚拟机远程连接

在实验五和实验六中已经完成 Ubuntu Server 虚拟机安装和网络配置，实验七中完成 SSH 远程登录。本实验继续使用虚拟机 `node2` 作为部署服务器，Host-Only 地址为 `192.168.56.102`。

在 Windows PowerShell 中远程登录虚拟机：

```bash
ssh dev@192.168.56.102
hostname
whoami
exit
```

![SSH 远程登录虚拟机](/images/exp8/exp8_03_ssh_remote.png)

*图3：通过 SSH 登录 node2 并验证主机名和用户*

### 步骤四：安装并启动 Nginx

在 Ubuntu Server 中安装 Nginx，并设置为立即启动和开机自启。

```bash
sudo apt update
sudo apt install -y nginx
sudo systemctl enable --now nginx
sudo systemctl status nginx
```

当状态显示 `active (running)` 时，说明 Nginx 服务已经正常运行。

![Nginx 服务运行状态](/images/exp8/exp8_04_nginx_status.png)

*图4：Ubuntu 虚拟机中 Nginx 服务 active (running)*

### 步骤五：配置 Nginx 静态站点目录

为了让 GitHub Pages 和虚拟机部署使用一致的访问路径，将站点部署到 `/var/www/blog/software-tools-practice/`。这样浏览器可以通过 `/software-tools-practice/` 子路径访问站点。

```bash
sudo mkdir -p /var/www/blog/software-tools-practice
sudo chown -R dev:dev /var/www/blog
```

创建 Nginx 站点配置 `/etc/nginx/sites-available/blog`：

```nginx
server {
    listen 80;
    server_name _;
    root /var/www/blog;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

启用配置并检查语法：

```bash
sudo ln -sf /etc/nginx/sites-available/blog /etc/nginx/sites-enabled/blog
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
```

![Nginx 配置检查成功](/images/exp8/exp8_05_nginx_test.png)

*图5：Nginx 配置语法检查成功*

### 步骤六：构建并同步静态站点

在本地 Hexo 项目中生成静态文件，然后通过 SCP 或 rsync 同步到虚拟机 Nginx 目录。

```bash
npm run build
scp -r public/* dev@192.168.56.102:/var/www/blog/software-tools-practice/
```

如果使用 rsync，可以执行：

```bash
rsync -avz --delete -e ssh public/ dev@192.168.56.102:/var/www/blog/software-tools-practice/
```

### 步骤七：浏览器访问验证

部署完成后，在宿主机浏览器中访问虚拟机站点：

```text
http://192.168.56.102/software-tools-practice/
```

页面能够显示课程展示站点，说明 Nginx 静态站点部署成功。

![虚拟机地址访问课程展示站点](/images/exp8/exp8_06_vm_site.png)

*图6：通过虚拟机地址访问课程展示站点*

同时访问 GitHub Pages 线上站点，确认自动部署版本也能正常显示实验八内容。

![GitHub Pages 线上站点显示实验八内容](/images/exp8/exp8_07_github_site.png)

*图7：GitHub Pages 线上站点显示实验八内容*

## 四、实验结果

| 测试项 | 结果 |
|---|---|
| GitHub Actions 自动构建 | 成功 |
| 静态文件发布到 `gh-pages` | 成功 |
| GitHub Pages 线上访问 | 成功 |
| SSH 远程登录虚拟机 | 成功 |
| Nginx 服务运行 | 成功 |
| Nginx 配置语法检查 | 成功 |
| 虚拟机站点访问 | 成功 |
| 页面样式与图片加载 | 成功 |

## 五、知识总结

| 知识点 | 说明 |
|---|---|
| GitHub Actions | 用于自动执行构建和部署流程，减少手动发布步骤 |
| GitHub Pages | 适合托管静态站点，可直接从仓库分支发布页面 |
| Hexo `public/` | Hexo 构建后的静态网页目录，可直接部署到 Web 服务器 |
| Nginx | 常用 Web 服务器，可以托管静态文件并提供浏览器访问 |
| SSH/SCP/rsync | 用于远程登录服务器和同步部署文件 |
| 站点根路径 | GitHub Pages 项目页和虚拟机部署都需要处理 `/software-tools-practice/` 子路径 |

## 六、出现问题与解决方法

| 问题 | 原因 | 解决方法 |
|---|---|---|
| GitHub 推送需要凭据 | 本地终端没有可用 GitHub 登录凭据 | 在已登录 GitHub 的终端执行 `git push` |
| GitHub Pages 显示 404 | 发布分支或目录未配置正确 | 将 Pages 来源设置为 `gh-pages` 分支 `/root` |
| 虚拟机页面样式或图片缺失 | 部署目录与 Hexo `root` 子路径不一致 | 将静态文件部署到 `/var/www/blog/software-tools-practice/` |
| Nginx 显示默认页 | 默认站点配置仍然启用 | 删除 `/etc/nginx/sites-enabled/default` 并重新加载 Nginx |

## 七、心得体会

实验八把前面完成的 Web 开发环境、Git 代码管理、Linux 虚拟机、SSH 远程管理全部串联起来，形成了一个可以真实访问的课程展示站点。通过 GitHub Pages 和 Nginx 两种部署方式的对比，可以看到自动化部署适合持续更新，Linux/Nginx 部署更接近真实服务器运维场景。遇到样式和图片加载问题时，也进一步理解了站点根路径、静态资源路径和服务器目录之间的关系。
