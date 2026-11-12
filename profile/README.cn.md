# TAFFISH

[English](README.md) | [中文](README.cn.md) · [官网](https://taffish.com)

**将可复现性带回 shell。**

TAFFISH 将生物信息学工具和轻量流程封装成带版本的命令。安装指定版本后，可以直接
用于已有的 shell 脚本、HPC 作业和工作流。TAFFISH 关注命令级可复现性，不替代你
使用的工作流系统。

<a id="目录"></a>
<a id="面向用户"></a>

## 从这里开始

<a id="主要入口"></a>

| 我想…… | 从这里进入 |
| --- | --- |
| 安装 TAFFISH，运行第一个 app | [快速开始](https://github.com/taffish/taffish-docs/blob/main/zh/quick-start.cn.md) |
| 查找工具、版本和安装命令 | [TAFFISH Hub](https://taffish.github.io/) |
| 查看流程和示例报告 | [Flow 门户](https://taffish.github.io/flows/) |
| 封装自己的工具 | [App 开发指南](https://github.com/taffish/taffish-docs/blob/main/zh/app-developer-guide.cn.md) |
| 浏览 TAFFISH 源代码 | [TAFFISH 核心仓库](https://github.com/taffish/taffish) |

<a id="flow-门户与案例"></a>

Flow 门户包含 RNA-seq、质量控制、BLAST 和系统发育等分析路线。
完整案例可从 [RNA-seq Flow 专题](https://taffish.github.io/rnaseq-flows/)开始。

<a id="taffish-提供什么"></a>
<a id="taffish-hub-如何运行"></a>

## 各部分如何配合

1. **App 描述一条命令。** `.taf` 源码描述执行过程，`taffish.toml` 记录身份、版本和
   依赖，帮助文档随 app 一起提供。Tool app 封装工具，Flow 则组合带版本的 app。
2. **Hub 关联发行版与资源。** 各 app 仓库发布版本，并在需要时构建自己的容器镜像。
   [Index](https://github.com/taffish/taffish-index)记录源码身份、镜像信息和 smoke 检查结果。
   Hub 网页和命令行都使用这份目录来发现可用版本。
3. **安装后的命令在你的环境中运行。** `taf` 安装选定发行版，创建带版本的命令。
   `taffish` 编译器将 TAF 源码转换为 shell；容器化命令调用 Docker、Podman 或
   Apptainer。各条命令如何组成分析，仍由你的脚本决定。

Hub 网页不执行你的分析。本地工具由 [TAFFISH 核心仓库](https://github.com/taffish/taffish)
提供，浏览界面由 [Hub 网页仓库](https://github.com/taffish/taffish.github.io)维护。
完整组件关系见[系统架构](https://github.com/taffish/taffish/blob/main/docs/architecture/README.zh-CN.md)。

<a id="安装入口"></a>
<a id="试用一个工具"></a>

## 安装与试用

按网络条件选择 [GitHub](#github)、[Gitee](#gitee) 或[本地离线安装](https://github.com/taffish/taffish/blob/main/README-CN.md#本地或离线安装)。
官方平台包提供 `taf`、`taffish` 和 `taffish-mcp`，安装和验证这三个命令无需另装 Lisp
或容器后端。下面的在线命令需要 `curl` 和网络连接。

| 支持平台 | 需额外准备 |
| --- | --- |
| Apple Silicon Mac，macOS 15 及以上 | 无 |
| 使用 glibc 的 Linux x86-64 | `tar`、`gzip` |

完整平台说明见[主仓库安装指南](https://github.com/taffish/taffish/blob/main/README-CN.md#快速安装)。

以下命令包含 `--accept-runtime-license`：在 Linux 上表示明确接受
[LispWorks 运行时条款](https://github.com/taffish/taffish/blob/main/licenses/LISPWORKS-RUNTIME.txt)，
无需中途输入 `yes`；请先阅读并接受条款再执行。如希望安装时手动确认，去掉该参数即可。
macOS 无需此确认，会忽略该参数。

个人安装仅供当前账号使用，无需 `sudo`；系统安装供整台设备的所有用户使用。四条命令只需执行一条。
系统安装需要管理员权限，`sudo` 仍可能要求输入密码。共享安装应安排使用者接受运行时条款，
或由具有相应授权的管理员代为接受。

### GitHub

**个人安装：**

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sh -s -- --user --accept-runtime-license
```

**系统安装：**

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sudo sh -s -- --system --accept-runtime-license
```

<a id="中国地区使用-gitee"></a>

### Gitee

GitHub 下载缓慢或不可用时，可以使用 Gitee；该入口会为新配置选用国内索引。
macOS 安装时会自动下载并合并压缩包分片；GitHub 和 Linux 下载仍使用完整压缩包。

**个人安装：**

```sh
curl -fsSL https://gitee.com/taffish-org/taffish/raw/main/install/install-taffish.gitee.sh | sh -s -- --user --accept-runtime-license
```

**系统安装：**

```sh
curl -fsSL https://gitee.com/taffish-org/taffish/raw/main/install/install-taffish.gitee.sh | sudo sh -s -- --system --accept-runtime-license
```

<a id="个人安装仅供当前用户使用"></a>
<a id="整台设备供所有用户使用"></a>
<a id="让新终端也能找到命令"></a>

### PATH 与安装验证

两种下载来源使用相同的默认路径：

- **个人安装：**核心命令位于 `~/.local/bin`，app 和本地 Hub 数据位于
  `~/.local/share/taffish`。执行 `export PATH="$HOME/.local/bin:$HOME/.local/share/taffish/bin:$PATH"`。
- **系统安装：**核心命令和共享 app 命令位于 `/usr/local/bin`，共享数据位于
  `/opt/taffish`。执行 `export PATH="/usr/local/bin:$PATH"`。

随后用 `taf --version` 验证。这些 export 只对当前 shell 会话及其子进程生效，不会保存到新终端。
个人安装时，将同一行 export 加入 `~/.zshrc`（Zsh）或 `~/.bashrc`（交互式 Bash）。
Bash 登录终端则使用 `~/.bash_profile`，或由它加载 `~/.bashrc`。保存后重新打开终端再次检查。
安装器不会自动修改 shell 配置文件。

共享安装由管理员确保各用户的 PATH 包含 `/usr/local/bin`。普通用户运行已安装 app 本身
不需要 `sudo`，但仍需满足容器后端的权限要求。

其他安装方式与维护细节见[本地离线安装](https://github.com/taffish/taffish/blob/main/README-CN.md#本地或离线安装)、
[固定版本](https://github.com/taffish/taffish/blob/main/install/README-CN.md#安装入口)、
[升级说明](https://github.com/taffish/taffish/blob/main/install/README-CN.md#升级和移除)和
[校验与验签](https://github.com/taffish/taffish/blob/main/install/README-CN.md#发布验签)。

### 试用 FastQC

试用容器化工具还需要 Git、网络连接，以及 Docker、Podman 或 Apptainer 中任意一个
可用后端。无论上面选择哪种核心安装方式，下面的示例都**仅为当前账号安装** FastQC，
再查看版本号，不需要准备输入数据。

如果个人 app 命令目录尚未加入 `PATH`，先执行
`export PATH="$HOME/.local/share/taffish/bin:$PATH"`。

```sh
taf update --user
taf install --user fastqc 0.12.1-r1
taf-fastqc-v0.12.1-r1 fastqc --version
```

系统级安装核心不会让后续 `taf` 命令自动使用系统范围。若要共享这个 app，应由管理员
改用 `sudo taf update --system` 和 `sudo taf install --system fastqc 0.12.1-r1`；
普通用户随后正常运行已安装命令即可。后端和缓存权限见
[共享安装指南](https://github.com/taffish/taffish-docs/blob/main/zh/system-administration.cn.md)。

最后一条命令会在容器中运行 FastQC，输出 `FastQC v0.12.1`。命令名固定了工具版本
（`0.12.1`）和封装修订号（`r1`）；首次运行可能需要下载镜像。

实际分析时，在存有你自己的 `reads.fq.gz` 的目录中，可以先查看将要执行的命令：

```sh
taf-fastqc-v0.12.1-r1 --compile fastqc reads.fq.gz
```

紧跟在封装命令名后的 `--compile` 会输出生成的 shell，包括容器镜像、挂载和工具参数，
不会运行 FastQC 或下载镜像。去掉 `--compile`，才会执行分析，并在输入文件旁生成
FastQC 报告：

```sh
taf-fastqc-v0.12.1-r1 fastqc reads.fq.gz
```

同一条带版本的命令也可以放进 shell 脚本或工作流系统。
编译预览用于检查执行方式，不代表数据或分析结果已经通过验证。

## 主要能力与指南

| 能力 | 可以做什么 | 详细说明 |
| --- | --- | --- |
| App 管理 | 查找、安装和升级工具，在脚本中使用明确版本。 | [Hub 使用指南](https://github.com/taffish/taffish-docs/blob/main/zh/taffish-hub.cn.md) |
| TAF 脚本 | 在 `.taf` 源码中描述和组合命令，再编译为 shell。 | [TAF 教程](https://github.com/taffish/taffish-docs/blob/main/zh/taf-script-tutorial.cn.md) |
| 容器后端 | 使用 Docker、Podman 或 Apptainer 运行容器化 app，具体支持情况以该 app 为准。 | [后端配置](https://github.com/taffish/taffish-docs/blob/main/zh/quick-start.cn.md#容器后端) |
| Flow 与报告 | 将带版本的 app 组合成可复用分析；部分流程还提供报告。只有自身实现需要时，Flow 才需另配容器。 | [Flow 指南](https://github.com/taffish/taffish-docs/blob/main/zh/taf-flow-developer-guide.cn.md) |
| 交互式 app | 部分 app 提供浏览器界面或桌面会话；后端、端口和访问要求见各 app 帮助。 | [示例：Cytoscape](https://github.com/taffish/cytoscape/blob/v3.10.5-r1/docs/help.md) |
| AI 客户端集成 | `taffish-mcp` 通过 MCP 提供检查、校验和编译能力，不执行分析或启动容器。 | [MCP 指南](https://github.com/taffish/taffish-docs/blob/main/zh/taffish-mcp.cn.md) |
| 验证信息 | 查看已记录的源码提交、镜像摘要和各后端 smoke 结果；这些检查不等于科学验证。 | [安全与信任模型](https://github.com/taffish/taffish-docs/blob/main/zh/security-model.cn.md) |
| 部署与诊断 | 配置个人或共享安装、镜像源和缓存，用 `taf doctor` 检查本地问题。 | [系统管理](https://github.com/taffish/taffish-docs/blob/main/zh/system-administration.cn.md) · [故障排查](https://github.com/taffish/taffish-docs/blob/main/zh/troubleshooting.cn.md) |

[文档总览](https://github.com/taffish/taffish-docs/blob/main/README.cn.md)提供完整用户与开发指南；
具体版本的变化见[核心发布记录](https://github.com/taffish/taffish/releases)。

<a id="面向-app-开发者"></a>

## 开发与参与

封装工具请读 [App 开发指南](https://github.com/taffish/taffish-docs/blob/main/zh/app-developer-guide.cn.md)，
组合流程请读 [Flow 开发指南](https://github.com/taffish/taffish-docs/blob/main/zh/taf-flow-developer-guide.cn.md)。
开发和使用自己的 app 不要求先发布到官方 Hub。

官方 Hub 目前由组织成员管理发布。推荐收录软件或讨论参与维护，请联系
[support@taffish.com](mailto:support@taffish.com)。软件问题请在对应仓库反馈；
核心问题可提交到 [taffish/taffish](https://github.com/taffish/taffish/issues)。

<a id="论文"></a>
<a id="项目状态"></a>

## 论文与开源

阅读 bioRxiv 预印本：
[TAFFISH: shell-native command-level reproducibility for bioinformatics](https://www.biorxiv.org/content/10.1101/2025.09.15.672424)。

作者：Kaiyuan Han、Ting Wang、Shishi Yuan、Caiyi Ma、Wei Su、Xiaolong Li、
Kejun Deng、Hao Lin 和 Hao Lyu。

核心实现以 [Apache License 2.0](https://github.com/taffish/taffish/blob/main/LICENSE) 开源；
封装的第三方工具保留各自的许可证。
TAFFISH 全称为 **Tools And Flows Framework Intensify SHell**。
