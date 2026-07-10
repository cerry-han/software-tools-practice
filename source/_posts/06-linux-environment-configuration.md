---
title: 实验六 Linux 环境配置
date: 2026-07-10 18:00:00
categories:
  - 实验记录
tags:
  - Linux
  - Ubuntu Server
  - Permission
  - Environment
---

实验六基于 Ubuntu Server 22.04 LTS 虚拟机环境，完成 Linux 基础命令、用户管理、权限控制和环境变量配置练习。

## 实验环境

| 项目 | 内容 |
|---|---|
| 操作系统 | Ubuntu Server 22.04 LTS |
| 虚拟化平台 | Oracle VirtualBox |
| 网络模式 | NAT + Host-Only |
| 主机名 | template |
| 登录用户 | dev |

## 核心操作

查看系统信息：

```bash
whoami
uname -a
lsb_release -a
hostname
pwd
```

文件与目录操作：

```bash
mkdir test_dir
touch test.txt
cp test.txt test_copy.txt
mv test.txt /tmp/
rm -f test_copy.txt
```

用户管理：

```bash
cat /etc/passwd
cat /etc/group
sudo adduser testuser
sudo usermod -aG sudo testuser
groups testuser
su - testuser
sudo deluser -r testuser
```

权限管理：

```bash
touch file1.txt file2.txt
chmod 755 file1.txt
chmod u+x file2.txt
ls -l
```

环境变量配置：

```bash
export MY_VAR="HelloWorld"
echo $MY_VAR
unset MY_VAR
nano ~/.bashrc
source ~/.bashrc
```

## 实验结果

- 成功查看 Ubuntu 系统版本、内核和主机名。
- 成功完成文件创建、复制、移动和删除。
- 成功创建 `testuser` 用户，并添加 sudo 权限。
- 成功使用数字法和符号法修改文件权限。
- 成功配置临时环境变量和用户级永久环境变量。

## 小结

实验六让小组熟悉了 Linux 服务器环境的基本管理方式。用户信息、组信息和环境变量均以配置文件形式保存，这种方式为后续自动化部署、远程管理和服务器维护提供了清晰基础。
