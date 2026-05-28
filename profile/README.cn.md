# TAFFISH

[English](README.md) | [中文](README.cn.md)

TAFFISH 将可复现性带回基于 Shell 的生物信息学命令。

TAFFISH 全称为 **Tools And Flows Framework Intensify SHell**。它是一个面向
生物信息学命令行工具与轻量流程的 shell-native 可复现执行交付层。

TAFFISH 不试图替代 shell 脚本或现有 workflow 系统。它把工具调用、参数、容器
运行环境、元数据和版本化 release 转化为可安装、可分发、可组合、可验证的可执行
包，同时仍然保持普通 shell 命令的使用形态。

## 目录

- [主要入口](#主要入口)
- [代表性 Flow 案例](#代表性-flow-案例)
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
| [taffish.com](https://taffish.com) | 官方项目主页，用于展示公开定位、项目故事、发展历史和公共入口。 |
| [TAFFISH 是什么？](https://taffish.com/stories/positioning/) | 核心定位说明：把可复现性带回基于 Shell 的生物信息学命令。 |
| [TAFFISH Hub](https://taffish.github.io) | 浏览当前可用的 TAFFISH 可执行包、apps、tools、flows、版本、依赖、可信元数据和安装命令。 |
| [RNA-seq Flow Family](https://taffish.github.io/rnaseq-flows/) | 第一个完整公开的 TAFFISH flow family 门户，包含流程手册和可在线查看的 yeast SNF2 示例报告。 |
| [taffish/taffish](https://github.com/taffish/taffish) | TAFFISH 开源源码仓库，包含安装器、源码树开发文档，以及 `taf`、`taffish` 和 `taffish-mcp` 的二进制 release 载荷。 |
| [taffish/taffish-docs](https://github.com/taffish/taffish-docs) | TAFFISH、TAFFISH Hub、app 项目、`.taf` 脚本、容器、依赖和发布流程文档。 |
| [TAFFISH 安全模型](https://github.com/taffish/taffish-docs/blob/main/zh/security-model.cn.md) | 面向 release、安装器、镜像、Hub/index gate、本地安装、容器和 MCP 的分层安全与可信模型。 |
| [taffish/taffish-index](https://github.com/taffish/taffish-index) | 由自动化生成的静态包索引，供 `taf update`、`taf search`、`taf info` 和 `taf install` 使用。 |
| [taffish/taffish.github.io](https://github.com/taffish/taffish.github.io) | 网页版 Hub 的源码仓库。 |
| [taffish/.github](https://github.com/taffish/.github) | 当前组织首页仓库。 |

## 代表性 Flow 案例

[TAFFISH RNA-seq Flow Family](https://taffish.github.io/rnaseq-flows/) 是第一个
完整公开的 TAFFISH flow family 案例。它展示了可版本化命令包如何组合成
shell-native、可审计的分析路线，同时不把 TAFFISH 写成传统 workflow engine。

这个门户包含：

- 参考构建、表达定量、差异表达、富集分析、可选比对/计数/QC 证据分支和报告的
  flow-family 地图；
- 每个 flow 的输入表、参数、输出和连接说明；
- 由标准 RNA-seq 路线生成的
  [yeast SNF2 在线报告](https://taffish.github.io/rnaseq-flows/examples/yeast-standard-report/04_reports/rnaseq_report.html)。

## TAFFISH 提供什么

- 面向生物信息学命令行的 shell-native 可复现执行层，从用户已经在使用的 Shell
  命令开始。
- shell-native 可执行包模型，将工具调用转化为可版本化、可容器解析、可安装、可分发、
  可组合、可验证的命令实体。
- 面向生物信息学工具与轻量 flows 的命令级可复现性，从任务命令下方的执行层
  补全 Nextflow、Snakemake 等 workflow 系统，而不是替代它们。
- TAFFISH DSL 语言及其 `taffish` 编译器，用于把 `.taf` 脚本编译为 Shell 脚本，并保留命令行工具的组合能力。
- `taf`，用于安装、更新、运行 TAFFISH 可执行包的本地包管理器、app 安装器与命令执行器。
- TAFFISH Hub，一个基于 GitHub 的可执行包生态，让用户可以发现、更新、安装、
  查询、维护并在本地运行 apps。
- 通过 Docker、Podman 或 Apptainer 进行 container-aware 执行，同时让 TAFFISH
  命令保持普通 shell 命令的使用形态。
- 面向版本、依赖、平台、容器、上游来源、smoke checks 和 Hub trust signals 的
  app 元数据。
- `taffish-mcp`，一个保守的 MCP server，让 AI 客户端可以检查 TAFFISH 项目，
  并在不运行容器的前提下编译候选命令。
- 使用 Apache License 2.0 开源的 Common Lisp 实现。

详细 CLI 行为、`.taf` 语法、容器运行参数、镜像配置、MCP 接入、安全细节、
源码构建和具体 release 变化，请阅读
[taffish/taffish-docs](https://github.com/taffish/taffish-docs) 和
[taffish/taffish README](https://github.com/taffish/taffish)。

## 安装入口

请从 [taffish/taffish](https://github.com/taffish/taffish) 安装 TAFFISH CLI。
该仓库是源码、当前安装器、支持平台、release 版本、运行依赖、容器 backend 说明、网络说明、源码构建、release 校验和故障排查的权威入口。

快速用户级安装：

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sh -s -- --user
```

系统级安装，适合共享服务器：

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sudo sh -s -- --system
```

中国大陆用户可以使用 Gitee 安装器，从 Gitee 镜像下载并初始化中国镜像 profile：

```sh
curl -fsSL https://gitee.com/taffish-org/taffish/raw/main/install/install-taffish.gitee.sh | sh -s -- --user
```

中国大陆系统级安装：

```sh
curl -fsSL https://gitee.com/taffish-org/taffish/raw/main/install/install-taffish.gitee.sh | sudo sh -s -- --system
```

中国/Gitee 说明：Gitee 镜像用于缓解 GitHub raw 内容较慢或被阻断的问题。部分镜像服务
可能限制大文件匿名 raw 下载，尤其是较大的 macOS 二进制文件。如果 Gitee 安装器无法
获取二进制文件，可以使用可用网络/代理访问 GitHub 安装器，登录镜像后手动下载，或者查看
当前 [taffish/taffish README](https://github.com/taffish/taffish) 中的说明。

本地可用 `taf` 之后，通常从这些命令开始：

```sh
taf doctor
taf update
taf search <keyword>
taf install <app>
taf info <app>
taf outdated
taf upgrade
taf list
```

固定版本安装、平台依赖、中国/Gitee 说明、镜像配置、源码构建、release 校验、
容器 backend 行为、私有/本地 app 安装和详细故障排查，请以当前
[taffish/taffish README](https://github.com/taffish/taffish) 和
[TAFFISH 文档](https://github.com/taffish/taffish-docs) 为准。

第一次使用可以阅读
[TAFFISH 快速开始](https://github.com/taffish/taffish-docs/blob/main/zh/quick-start.cn.md)。

## TAFFISH Hub 如何运行

TAFFISH Hub 当前基于 GitHub 实现。它发布供本地 `taf` 命令消费的可执行包元数据，
而不是在服务器上运行用户任务。

1. 每个 app 都位于独立仓库，根目录包含 `taffish.toml`。
2. app 仓库发布类似 `v0.1.0-r1` 的 release tag。
3. app 仓库通过自己的 GitHub Actions workflow 构建容器镜像。
4. [taffish-index](https://github.com/taffish/taffish-index) 扫描组织、校验新增版本、记录依赖、平台、容器 digest、smoke、trust 和 upstream 元数据，并生成静态 JSON 索引。
5. 用户执行 `taf update` 后，就可以在本地通过 `taf` 安装和运行 apps。

[taffish.github.io](https://taffish.github.io) 是面向人的网页版 Hub；
[taffish-index](https://github.com/taffish/taffish-index) 是面向程序消费的数据源。

## 面向用户

可以通过 [TAFFISH Hub](https://taffish.github.io) 浏览可用可执行包，并在本地通过
`taf` 安装和运行它们。关于安装、CLI 行为、`.taf` 语法、容器使用和故障排查，请阅读
[taffish/taffish-docs](https://github.com/taffish/taffish-docs)。

## 面向 App 开发者

TAFFISH app 是结构化的可执行包仓库，通常包含 `taffish.toml`、`src/main.taf`、
`docs/help.md`、`target/` 构建产物、版本化 release tag，并且可以提供
dependencies、platform、container、smoke checks、Hub trust status 和 upstream source 等可选元数据。

官方 Hub 当前由 `taffish` 组织维护。现阶段，发布到官方 Hub 仅限组织成员。如果
开发者希望贡献 app，可以联系维护者讨论加入组织或具体发布方式。

## 论文

TAFFISH 项目已在 bioRxiv 预印本中描述：

[TAFFISH: A lightweight, modular, and containerized workflow framework for reproducible bioinformatics analyses](https://www.biorxiv.org/content/10.1101/2025.09.15.672424v2)

作者：Kaiyuan Han、Ting Wang、Shi-Shi Yuan、Cai-Yi Ma、Wei Su、Kejun Deng、
Xiaolong Li、Hao Lv 和 Hao Lin。

## 项目状态

TAFFISH 已经开源，并且仍在积极开发中。本地 CLI/编译器实现位于
[taffish/taffish](https://github.com/taffish/taffish)，使用 Apache License 2.0
授权。当前公开基础设施主要基于 GitHub repositories、GitHub Actions、GitHub
Packages、GitHub Pages 和静态 package index。未来也许会需要独立服务器版本的
Hub，但当前设计会优先保持发布和索引路径简单、透明、容易复现。
