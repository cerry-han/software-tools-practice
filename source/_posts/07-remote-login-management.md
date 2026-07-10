---
title: 实验七 远程登录管理
date: 2026-07-10 18:30:00
categories:
  - 实验记录
tags:
  - SSH
  - Linux
  - SCP
  - Remote
---

实验七基于 Ubuntu Server 多机环境，完成 OpenSSH 服务配置、SSH 远程登录、密钥免密登录和 SCP 文件传输验证。

## 实验环境

| 节点 | 主机名 | IP | 用途 |
|---|---|---|---|
| node1 | node1 | 192.168.56.101 | SSH 客户端 |
| node2 | node2 | 192.168.56.102 | SSH 服务端 |

## SSH 服务配置

在 node2 上安装并启动 SSH 服务：

```bash
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
sudo systemctl status ssh
```

## 密码登录

在 node1 上连接 node2：

```bash
ssh dev@192.168.56.102
```

登录成功后执行：

```bash
hostname
whoami
exit
```

## 免密登录

在 node1 上生成密钥并复制到 node2：

```bash
ssh-keygen -t ed25519 -C "software-tools-practice"
ssh-copy-id dev@192.168.56.102
ssh dev@192.168.56.102
```

## 文件传输

使用 SCP 将测试文件从 node1 传输到 node2：

```bash
echo "software tools practice ssh test" > ssh_test.txt
scp ssh_test.txt dev@192.168.56.102:/home/dev/
```

在 node2 上验证文件：

```bash
cat /home/dev/ssh_test.txt
```

## 小结

实验七完成后，小组具备了通过 SSH 远程管理 Linux 服务器的能力，也验证了密钥认证、SCP 文件传输、rsync 目录同步和基于 SSH 的 Git 远程仓库流程。这为后续 Web 服务部署、远程同步和服务器维护提供了基础。
