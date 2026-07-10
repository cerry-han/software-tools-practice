---
title: 实验四 Web 站点构建
date: 2026-07-10 16:10:00
categories:
  - 实验记录
tags:
  - Hexo
  - Static Site
  - Blog
---

实验四在实验二的 Hexo 项目基础上，将默认博客改造成课程实验展示站点。

## 构建内容

- 修改站点标题、作者、语言和描述。
- 删除默认 Hello World 文章。
- 新增课程项目总览、小组分工、实验二、实验三和实验四文章。
- 将实验截图复制到 `source/images/` 目录。
- 使用 `hexo clean` 和 `hexo generate` 重新生成站点。

## 站点结构

```text
source/
├── _posts/
│   ├── 00-course-overview.md
│   ├── 01-team-division.md
│   ├── 02-web-development-environment.md
│   ├── 03-code-editor-markdown.md
│   └── 04-web-site-construction.md
└── images/
    ├── exp2/
    └── exp3/
```

## 小结

通过实验四，项目从默认 Hexo 博客变成了面向课程成果展示的静态站点雏形。后续实验五至实验八的内容可以继续以文章形式补充到本站点中。

