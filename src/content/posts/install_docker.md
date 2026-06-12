---
title: 在Rocky Linux上安装Docker
published: 2026-06-12
description: '教学如何在Rocky Linux上安装Docker'
image: ''
tags: ['docker', 'linux']
category: 'docker'
draft: false 
lang: 'zh-CN'
---

## 前言

Rocky Linux 是一个企业级的开源 Linux 发行版，由 Gregory Kurtzer 于 2021 年创立。它诞生于 CentOS 项目转向 CentOS Stream 之后，旨在成为 CentOS 的完全兼容替代品，提供与 Red Hat Enterprise Linux (RHEL) 二进制兼容的稳定服务器环境。Rocky Linux 由 Rocky Enterprise Software Foundation (RESF) 维护，采用社区驱动的方式开发，完全免费且开源，继承了 RHEL 的稳定性和可靠性，非常适合生产环境使用。

> 主要是，这个相当于免费红帽，所以我一直在用这个

话不多说，教学开始！

---

## 目录

1. [安装 EPEL 仓库](#1-安装-epel-仓库)
2. [安装 Docker 运行所需工具](#2-安装-docker-运行所需工具)
3. [添加 Docker 官方仓库](#3-添加-docker-官方仓库)
4. [安装 Docker](#4-安装-docker)
5. [启动 Docker 服务](#5-启动-docker-服务)
6. [配置 Docker 代理](#6-配置-docker-代理)
7. [测试 Docker 镜像拉取](#7-测试-docker-镜像拉取)
8. [创建与管理容器](#8-创建与管理容器)
9. [Ubuntu 容器内部安装网络工具](#9-ubuntu-容器内部安装网络工具)
10. [验证 Docker](#10-验证-docker)

---

## 1. 安装 EPEL 仓库

```bash
sudo dnf install epel-release -y
```

---

## 2. 安装 Docker 运行所需工具

启用 CRB 仓库：

```bash
sudo dnf config-manager --set-enabled crb
```

安装 Docker 运行所需依赖：

```bash
sudo dnf install -y yum-utils device-mapper-persistent-data lvm2
```

| 依赖包 | 说明 |
|--------|------|
| `yum-utils` | 提供 yum/dnf 扩展工具 |
| `device-mapper-persistent-data` | Docker 存储相关依赖 |
| `lvm2` | 逻辑卷管理支持 |

---

## 3. 添加 Docker 官方仓库

添加 Docker CE 软件源：

```bash
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

验证仓库是否添加成功：

```bash
sudo dnf repolist | grep docker
```

---

## 4. 安装 Docker

安装 Docker CE 及相关组件：

```bash
sudo dnf install docker-ce docker-ce-cli containerd.io -y
```

| 组件 | 说明 |
|------|------|
| `docker-ce` | Docker 服务端 |
| `docker-ce-cli` | Docker 命令行工具 |
| `containerd.io` | 容器运行时 |

---

## 5. 启动 Docker 服务

启动 Docker：

```bash
sudo systemctl start docker
```

查看 Docker 运行状态：

```bash
systemctl status docker
```

---

## 6. 配置 Docker 代理

由于拉取 Docker Hub 镜像可能速度较慢，因此需要配置代理。

> [!NOTE]
> 这里只做教程，不提供代理工具

创建 Docker 服务配置目录：

```bash
sudo mkdir -p /etc/systemd/system/docker.service.d
```

创建代理配置文件：

```bash
sudo tee /etc/systemd/system/docker.service.d/proxy.conf <<-'EOF'
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
Environment="NO_PROXY=localhost,127.0.0.1"
EOF
```

重新加载 systemd 配置并重启 Docker：

```bash
sudo systemctl daemon-reexec
sudo systemctl restart docker
```

---

## 7. 测试 Docker 镜像拉取

搜索 Ubuntu 镜像：

```bash
sudo docker search ubuntu
```

拉取 Ubuntu 最新版本：

```bash
sudo docker pull ubuntu:latest
```

查看本地镜像：

```bash
sudo docker images
```

输出示例：

```
REPOSITORY   TAG       IMAGE ID       SIZE
ubuntu       latest    xxxxxxxxxxxx   xxMB
```

---

## 8. 创建与管理容器

### 8.1 创建容器

创建第一个 Ubuntu 容器：

```bash
sudo docker run -it --name ubuntu1 ubuntu:latest bash
```

创建多个测试节点：

```bash
sudo docker run -it --name ubuntu2 ubuntu:latest bash
sudo docker run -it --name ubuntu3 ubuntu:latest bash
```

参数说明：

| 参数 | 说明 |
|------|------|
| `-i` | 保持交互输入 |
| `-t` | 分配终端 |
| `--name` | 指定容器名称 |
| `bash` | 进入容器终端 |

### 8.2 管理容器

查看已有容器：

```bash
sudo docker ps -a
```

停止容器：

```bash
sudo docker stop <容器名>
```

删除容器：

```bash
sudo docker rm <容器名>
```

启动已存在的容器：

```bash
sudo docker start ubuntu1
```

进入已启动的容器：

```bash
sudo docker exec -it ubuntu1 bash
```

---

## 9. Ubuntu 容器内部安装网络工具

进入容器后，更新软件源并安装网络工具：

```bash
apt update && apt install -y iproute2 net-tools
```

安装后可使用以下命令：

| 命令 | 说明 |
|------|------|
| `ip addr` | 查看 IP 地址 |
| `ifconfig` | 查看网络配置 |

---

## 10. 验证 Docker

查看运行中的容器：

```bash
sudo docker ps
```

查看全部容器（含已停止）：

```bash
sudo docker ps -a
```

查看 Docker 系统信息：

```bash
docker info
```

---

至此，Docker 环境部署完成！ 🎉
