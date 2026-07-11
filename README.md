# 软件开发工具实践课程项目

本仓库为“软件开发工具实践”短学期课程小组项目源码。项目基于 Hexo 构建静态展示站点，用于展示课程实验二至实验八的实践过程、关键截图、问题总结和部署结果。

## 小组信息

小组：第六组

小组人数：6 人

| 成员 | 分工 |
|---|---|
| 何银尧 | 组长/集成负责人：统筹进度、搭建展示站点、整合报告、最终提交 |
| 龚嫣然 | 远程管理：SSH 服务、免密登录、SCP/rsync 文件传输、远程 Git 仓库验证 |
| 董晗 | Web 环境与 Hexo：Node.js、npm、Git、Hexo |
| 谢娩婷 | 文档与 VS Code：Markdown、插件 |
| 任鑫瑜 | Linux/虚拟机：VirtualBox、Ubuntu Server、Linux 命令、环境变量 |
| 程奕菲 | 部署与验收：GitHub Pages、GitHub Actions、Nginx/虚拟机部署 |

## 技术栈

- VS Code
- Markdown
- Git / GitHub
- Node.js / npm
- Hexo
- Ubuntu Server
- SSH / SCP / rsync
- GitHub Pages 或 Nginx

## 本地运行

```bash
npm install
npm run clean
npm run build
npm run server
```

访问：

```text
http://localhost:4000
```

## 提交说明

最终提交包括：

- 整合版实验报告 PDF
- 项目源码包
- 源码仓库链接
- 展示网站访问地址
- 小组分工说明
- 关键运行截图
