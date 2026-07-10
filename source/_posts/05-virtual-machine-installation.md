---
title: 实验五 虚拟机安装与使用
date: 2026-07-10 17:30:00
categories:
  - 实验记录
tags:
  - VirtualBox
  - Ubuntu Server
  - Linux
  - Host-Only
---

实验五的目标是在 VirtualBox 中安装 Ubuntu Server 22.04 LTS，并通过模板机克隆构建多机虚拟化环境，为后续 Linux 配置、SSH 远程管理和 Web 部署实验准备基础环境。

本实验已根据小组成员完成的实验五报告整理，原始材料包含虚拟机创建、网络配置、克隆节点和连通性验证过程。

## 实验目标

- 安装 VirtualBox 和 Ubuntu Server 22.04 LTS。
- 创建 `ubuntu-template` 模板机。
- 配置 NAT + Host-Only 双网卡。
- 设置模板机静态 IP 为 `192.168.56.101`。
- 克隆生成 `node1`、`node2`、`node3`。
- 验证虚拟机访问外网和虚拟机之间互通。

## 节点规划

| 节点 | 主机名 | Host-Only IP |
|---|---|---|
| 模板机 | template | 192.168.56.101 |
| 节点 1 | node1 | 192.168.56.102 |
| 节点 2 | node2 | 192.168.56.103 |
| 节点 3 | node3 | 192.168.56.104 |

## 网络配置

模板机 netplan 配置如下：

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: yes
    enp0s8:
      dhcp4: no
      addresses:
        - 192.168.56.101/24
      nameservers:
        addresses: [8.8.8.8, 114.114.114.114]
```

克隆机只需要把 Host-Only 地址分别改为 `192.168.56.102/24`、`192.168.56.103/24` 和 `192.168.56.104/24`。

## 验证命令

在 node1 中执行：

```bash
ping -c 3 8.8.8.8
ping -c 3 192.168.56.1
ping -c 3 node2
ping -c 3 node3
```

验证结果显示，node1 可访问外网、宿主机地址，并可通过 Host-Only 网络访问 node2 和 node3。宿主机到 node1、node1 到 node2 的 SSH 远程登录也验证成功。

## 小结

实验五完成后，课程项目将具备可复用的多机 Linux 实验环境。后续实验六、实验七和实验八都可以直接基于该虚拟机环境继续开展。
