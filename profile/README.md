# TAFFISH

TAFFISH is an open bioinformatics tool and workflow ecosystem for building,
sharing, and running reproducible analyses with versioned apps, containerized
runtime environments, and a lightweight shell-native workflow language.

It sits between ad-hoc shell scripts and heavyweight workflow systems: close
enough to the command line for everyday bioinformatics work, but structured
enough to make tools, environments, parameters, and workflows reusable.

[中文说明](#中文说明)

## Start Here

| Resource | Purpose |
| --- | --- |
| [TAFFISH Hub](https://taffish.github.io) | Browse available TAFFISH apps, tools, and flows. |
| [taffish/taffish](https://github.com/taffish/taffish) | Core CLI, compiler, and language implementation. Source release is in preparation. |
| [taffish/taffish-docs](https://github.com/taffish/taffish-docs) | Developer documentation for TAFFISH, TAFFISH Hub, app projects, and publishing. |
| [taffish/taffish-index](https://github.com/taffish/taffish-index) | Generated static package index consumed by `taf update` and `taf install`. |
| [taffish/.github](https://github.com/taffish/.github) | This organization profile. |

## What TAFFISH Provides

- A small workflow language for composing bioinformatics tools and flows.
- `taf`, a local package manager and runner for TAFFISH apps.
- Versioned app releases using the `version-release` form, such as `0.1.0-r1`.
- Container-aware execution through Docker or Podman backends.
- Hub indexing so users can discover, update, install, and inspect apps from a static GitHub-hosted index.
- Flow dependency metadata so `taf install` can resolve required app versions automatically.

## How the Hub Works

TAFFISH Hub is currently GitHub-based.

1. Each app lives in its own repository with a root `taffish.toml`.
2. App repositories publish release tags such as `v0.1.0-r1`.
3. App repositories build container images in their own GitHub Actions workflows.
4. [taffish-index](https://github.com/taffish/taffish-index) scans the organization and writes static JSON index files.
5. Users run `taf update`, then install and run apps locally through `taf`.

The public web Hub at [taffish.github.io](https://taffish.github.io) is the
human-facing browser for this ecosystem. The index repository is the
machine-facing data source.

## For Users

Typical usage starts from the local `taf` command:

```sh
taf update
taf install <app>
taf list --online
taf info <app>
```

Installation details, backend configuration, and language usage belong in the
core project and documentation repositories rather than this organization
profile.

## For App Developers

TAFFISH app projects are structured repositories with `taffish.toml`,
`src/main.taf`, `docs/help.md`, versioned release tags, and optional metadata for
dependencies, platform constraints, containers, and upstream software sources.

The official Hub is curated by the `taffish` organization. At the moment,
publishing to the official Hub is limited to organization members. Developers
who want to contribute apps can contact the maintainer to discuss joining the
organization or publishing strategy.

## Paper

The TAFFISH project is described in the bioRxiv preprint:

[TAFFISH: A lightweight, modular, and containerized workflow framework for reproducible bioinformatics analyses](https://www.biorxiv.org/content/10.1101/2025.09.15.672424v2)

Authors: Kaiyuan Han, Ting Wang, Shi-Shi Yuan, Cai-Yi Ma, Wei Su, Kejun Deng,
Hao Lv, and Hao Lin.

## Project Status

TAFFISH is under active development. The current public infrastructure is built
around GitHub repositories, GitHub Actions, GitHub Packages, GitHub Pages, and a
static package index. A dedicated server-backed Hub may become useful later, but
the current design intentionally keeps the publishing and indexing path simple,
auditable, and easy to reproduce.

---

## 中文说明

TAFFISH 是一个面向生物信息学工具与流程的开放生态，用于构建、分享和运行可复现
的数据分析。它把版本化 app、容器化运行环境，以及一个轻量的 shell-native 工作流
语言组合在一起。

TAFFISH 的定位介于临时 shell 脚本和重型 workflow 系统之间：它尽量保持命令行的
直接性，同时为工具、环境、参数和流程提供可复用的结构。

## 主要入口

| 资源 | 作用 |
| --- | --- |
| [TAFFISH Hub](https://taffish.github.io) | 浏览当前可用的 TAFFISH apps、tools 和 flows。 |
| [taffish/taffish](https://github.com/taffish/taffish) | 核心 CLI、编译器与语言实现。源码发布仍在准备中。 |
| [taffish/taffish-docs](https://github.com/taffish/taffish-docs) | 面向开发者的文档，包括 TAFFISH、TAFFISH Hub、app 项目与发布流程。 |
| [taffish/taffish-index](https://github.com/taffish/taffish-index) | 由自动化生成的静态包索引，供 `taf update` 和 `taf install` 使用。 |
| [taffish/.github](https://github.com/taffish/.github) | 当前组织首页仓库。 |

## TAFFISH 提供什么

- 一个用于组合生信工具和流程的小型 workflow language。
- `taf`，用于安装、更新、运行 TAFFISH apps 的本地包管理器与执行器。
- 类似 `0.1.0-r1` 的 version-release 版本体系。
- 通过 Docker 或 Podman backend 进行 container-aware 执行。
- 基于 Hub index 的 app 发现、更新、安装和信息查询。
- flow dependency 元数据，使 `taf install` 可以自动解析所需 app 版本。

## Hub 如何运行

TAFFISH Hub 当前基于 GitHub 实现。

1. 每个 app 都位于独立仓库，根目录包含 `taffish.toml`。
2. app 仓库发布类似 `v0.1.0-r1` 的 release tag。
3. app 仓库通过自己的 GitHub Actions workflow 构建容器镜像。
4. [taffish-index](https://github.com/taffish/taffish-index) 扫描组织并生成静态 JSON 索引。
5. 用户执行 `taf update` 后，就可以在本地通过 `taf` 安装和运行 apps。

[taffish.github.io](https://taffish.github.io) 是面向人的网页版 Hub；
[taffish-index](https://github.com/taffish/taffish-index) 是面向程序消费的数据源。

## 面向用户

常见使用从本地 `taf` 命令开始：

```sh
taf update
taf install <app>
taf list --online
taf info <app>
```

具体安装方式、backend 配置和语言使用细节，应进入核心项目和文档仓库继续查看，而
不是放在组织首页中展开。

## 面向 App 开发者

TAFFISH app 是结构化仓库，通常包含 `taffish.toml`、`src/main.taf`、
`docs/help.md`、版本化 release tag，并且可以提供 dependencies、platform、
container 和 upstream source 等可选元数据。

官方 Hub 当前由 `taffish` 组织维护。现阶段，发布到官方 Hub 仅限组织成员。如果
开发者希望贡献 app，可以联系维护者讨论加入组织或具体发布方式。

## 论文

TAFFISH 项目已在 bioRxiv 预印本中描述：

[TAFFISH: A lightweight, modular, and containerized workflow framework for reproducible bioinformatics analyses](https://www.biorxiv.org/content/10.1101/2025.09.15.672424v2)

作者：Kaiyuan Han、Ting Wang、Shi-Shi Yuan、Cai-Yi Ma、Wei Su、Kejun Deng、
Hao Lv 和 Hao Lin。

## 项目状态

TAFFISH 正在积极开发中。当前公开基础设施主要基于 GitHub repositories、GitHub
Actions、GitHub Packages、GitHub Pages 和静态 package index。未来也许会需要独立
服务器版本的 Hub，但当前设计会优先保持发布和索引路径简单、透明、容易复现。
