# TAFFISH

[English](README.md) | [中文](README.cn.md)

TAFFISH 是一个面向生物信息学工具与流程的开放生态，用于构建、分享和运行可复现
的数据分析。它把版本化 app、容器化运行环境，以及一个轻量的 shell-native 工作流
语言组合在一起。

TAFFISH 的定位介于临时 shell 脚本和重型 workflow 系统之间：它尽量保持命令行的
直接性，同时为工具、环境、参数和流程提供可复用的结构。

## 目录

- [主要入口](#主要入口)
- [TAFFISH 提供什么](#taffish-提供什么)
- [安装入口](#安装入口)
- [TAFFISH Hub 如何运行](#taffish-hub-如何运行)
- [面向用户](#面向用户)
- [面向 App 开发者](#面向-app-开发者)
- [论文](#论文)
- [项目状态](#项目状态)

## 主要入口

| 资源 | 作用 |
| --- | --- |
| [taffish.com](https://taffish.com) | 官方项目主页，用于展示项目故事、发展历史和公共入口。 |
| [TAFFISH Hub](https://taffish.github.io) | 浏览当前可用的 TAFFISH apps、tools、flows、版本、依赖和安装命令。 |
| [taffish/taffish](https://github.com/taffish/taffish) | 本地 `taf` 和 `taffish` 命令的二进制分发仓库，也是当前权威安装入口。 |
| [taffish/taffish-docs](https://github.com/taffish/taffish-docs) | TAFFISH、TAFFISH Hub、app 项目、`.taf` 脚本、容器、依赖和发布流程文档。 |
| [taffish/taffish-index](https://github.com/taffish/taffish-index) | 由自动化生成的静态包索引，供 `taf update`、`taf search`、`taf info` 和 `taf install` 使用。 |
| [taffish/taffish.github.io](https://github.com/taffish/taffish.github.io) | 网页版 Hub 的源码仓库。 |
| [taffish/.github](https://github.com/taffish/.github) | 当前组织首页仓库。 |

## TAFFISH 提供什么

- 一个用于组合生信工具和流程的小型 workflow language。
- `taf`，用于安装、更新、运行 TAFFISH apps 的本地包管理器与执行器。
- 类似 `0.1.0-r1` 的 version-release 版本体系。
- 通过 Apptainer、Podman 或 Docker backend 进行 container-aware 执行。
- 基于 Hub index 的 app 发现、更新、安装和信息查询。
- flow dependency 元数据，使 `taf install` 可以自动解析所需 app 版本。
- 支持中国/Gitee 或内部 Git 服务镜像的运行时镜像配置。

## 安装入口

请从 [taffish/taffish](https://github.com/taffish/taffish) 安装 TAFFISH CLI。
该仓库是当前安装器、支持平台、运行依赖、容器 backend 说明和故障排查的权威入口。

快速用户级安装：

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sh -s -- --user
```

固定安装 0.2.0：

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sh -s -- --version 0.2.0 --user
```

中国大陆用户可以使用 Gitee 安装器，从 Gitee 镜像下载并初始化中国镜像 profile：

```sh
curl -fsSL https://gitee.com/taffish-org/taffish/raw/main/install/install-taffish.gitee.sh | sh -s -- --user
```

本地可用 `taf` 之后，通常从这些命令开始：

```sh
taf doctor
taf update
taf search <keyword>
taf install <app>
taf info <app>
taf list
```

TAFFISH `0.2.0` 也支持持久化镜像配置：

```sh
taf config init --china --force
taf update
```

运行时配置可以改变 `taf update` 使用的 index URL，并在 `taf install` 时重写
canonical GitHub app 仓库 URL。因此可以在不改变官方 index schema 的情况下使用
Gitee 或内部 Git 服务镜像。

第一次使用可以阅读
[TAFFISH 快速开始](https://github.com/taffish/taffish-docs/blob/main/zh/quick-start.cn.md)。

## TAFFISH Hub 如何运行

TAFFISH Hub 当前基于 GitHub 实现。

1. 每个 app 都位于独立仓库，根目录包含 `taffish.toml`。
2. app 仓库发布类似 `v0.1.0-r1` 的 release tag。
3. app 仓库通过自己的 GitHub Actions workflow 构建容器镜像。
4. [taffish-index](https://github.com/taffish/taffish-index) 扫描组织并生成静态 JSON 索引。
5. 用户执行 `taf update` 后，就可以在本地通过 `taf` 安装和运行 apps。

[taffish.github.io](https://taffish.github.io) 是面向人的网页版 Hub；
[taffish-index](https://github.com/taffish/taffish-index) 是面向程序消费的数据源。

## 面向用户

可以通过 [TAFFISH Hub](https://taffish.github.io) 浏览可用 app，并在本地通过
`taf` 安装和运行它们。关于安装、CLI 行为、`.taf` 语法、容器使用和故障排查，请阅读
[taffish/taffish-docs](https://github.com/taffish/taffish-docs)。

## 面向 App 开发者

TAFFISH app 是结构化仓库，通常包含 `taffish.toml`、`src/main.taf`、
`docs/help.md`、`target/` 构建产物、版本化 release tag，并且可以提供
dependencies、platform、container 和 upstream source 等可选元数据。

官方 Hub 当前由 `taffish` 组织维护。现阶段，发布到官方 Hub 仅限组织成员。如果
开发者希望贡献 app，可以联系维护者讨论加入组织或具体发布方式。

## 论文

TAFFISH 项目已在 bioRxiv 预印本中描述：

[TAFFISH: A lightweight, modular, and containerized workflow framework for reproducible bioinformatics analyses](https://www.biorxiv.org/content/10.1101/2025.09.15.672424v2)

作者：Kaiyuan Han、Ting Wang、Shi-Shi Yuan、Cai-Yi Ma、Wei Su、Kejun Deng、
Xiaolong Li、Hao Lv 和 Hao Lin。

## 项目状态

TAFFISH 正在积极开发中。当前公开基础设施主要基于 GitHub repositories、GitHub
Actions、GitHub Packages、GitHub Pages 和静态 package index。未来也许会需要独立
服务器版本的 Hub，但当前设计会优先保持发布和索引路径简单、透明、容易复现。
