# TAFFISH

[English](README.md) | [中文](README.cn.md)

TAFFISH brings reproducibility back to shell-based bioinformatics commands.

TAFFISH stands for **Tools And Flows Framework Intensify SHell**. It is a
shell-native reproducible execution and delivery layer for bioinformatics
command-line tools and lightweight workflows.

TAFFISH does not try to replace shell scripting or existing workflow systems.
Instead, it turns tool calls, parameters, container runtimes, metadata, and
versioned releases into installable, distributable, composable, and verifiable
executable packages that still behave like ordinary shell commands.

## Table of Contents

- [Start Here](#start-here)
- [What TAFFISH Provides](#what-taffish-provides)
- [Installation](#installation)
- [How TAFFISH Hub Works](#how-taffish-hub-works)
- [For Users](#for-users)
- [For App Developers](#for-app-developers)
- [Paper](#paper)
- [Project Status](#project-status)

## Start Here

| Resource | Purpose |
| --- | --- |
| [taffish.com](https://taffish.com) | Official project homepage, public positioning, project story, history, and entry point. |
| [What Is TAFFISH?](https://taffish.com/stories/positioning/) | Core positioning note: bringing reproducibility back to shell-based bioinformatics commands. |
| [TAFFISH Hub](https://taffish.github.io) | Browse available TAFFISH executable packages, apps, tools, flows, versions, dependencies, trust metadata, and install commands. |
| [taffish/taffish](https://github.com/taffish/taffish) | Open-source TAFFISH source repository, installers, source-tree developer docs, and binary release payloads for `taf`, `taffish`, and `taffish-mcp`. |
| [taffish/taffish-docs](https://github.com/taffish/taffish-docs) | Documentation for TAFFISH, TAFFISH Hub, app projects, `.taf` scripts, containers, dependencies, and publishing. |
| [TAFFISH Security Model](https://github.com/taffish/taffish-docs/blob/main/en/security-model.en.md) | Layered security and trust model for releases, installers, mirrors, Hub/index gates, local installs, containers, and MCP. |
| [taffish/taffish-index](https://github.com/taffish/taffish-index) | Generated static package index consumed by `taf update`, `taf search`, `taf info`, and `taf install`. |
| [taffish/taffish.github.io](https://github.com/taffish/taffish.github.io) | Source repository for the web Hub. |
| [taffish/.github](https://github.com/taffish/.github) | This organization profile. |

## What TAFFISH Provides

- A shell-native reproducible execution layer for command-line bioinformatics,
  starting from the shell commands users already run.
- A shell-native executable package model that turns tool invocations into
  versioned, container-resolved, installable, distributable, composable, and
  verifiable command entities.
- Command-level reproducibility for bioinformatics tools and lightweight flows,
  complementing workflow systems such as Nextflow and Snakemake from the layer
  beneath their task commands rather than replacing them.
- The TAFFISH DSL and its `taffish` compiler, which compile `.taf` scripts into shell scripts while preserving command-line composability.
- `taf`, a local package manager, app installer, and command runner for TAFFISH executable packages.
- Open-source Common Lisp implementation under Apache License 2.0, with source-build, contribution, and security documentation in [taffish/taffish](https://github.com/taffish/taffish).
- A layered security model covering release payload verification, source commit checks, container digest/platform metadata, smoke gates, and conservative MCP boundaries.
- Versioned app releases using the `version-release` form, such as `0.1.0-r1`.
- Container-aware execution through Apptainer, Podman, or Docker backends.
- Runtime backend overrides for generic container tags through `TAFFISH_CONTAINER_BACKEND`.
- Backend-specific container runtime arguments in `.taf` tags through
  `$@[target: args]`, for app requirements such as GPU flags, host networking,
  or backend-specific devices.
- Local backend runtime-argument environment variables:
  `TAFFISH_DOCKER_RUN_ARGS`, `TAFFISH_PODMAN_RUN_ARGS`, and
  `TAFFISH_APPTAINER_RUN_ARGS`, for per-run or site-specific policies that are
  not part of the app implementation.
- Hub indexing so users can discover, update, install, and inspect apps from a static GitHub-hosted index.
- Smoke metadata and Hub-side trust gates for containerized apps, including container digest, platform metadata, and smoke status in the index.
- Flow dependency metadata so `taf install` can resolve required app versions automatically.
- Private/local project installation through `taf install --from` for apps that are not yet published to the public Hub.
- `taf publish --release`, backed by ignored `release.md` drafts for publish messages and GitHub Release notes.
- Runtime mirror configuration for China/Gitee or internal Git service mirrors.
- Installer-managed shell completion files and Vim syntax files for everyday CLI and `.taf` editing.
- `taffish-mcp`, a conservative stdio MCP server for safe AI-client access to TAFFISH project, app, Hub, config, history, resources, prompts, read-only TAF compiler helpers, and safe app/project compile previews. See the [TAFFISH MCP Guide](https://github.com/taffish/taffish-docs/blob/main/en/taffish-mcp.en.md) and [AI client setup guide](https://github.com/taffish/taffish-docs/blob/main/en/mcp-clients.en.md).

## Installation

Install the TAFFISH CLI from [taffish/taffish](https://github.com/taffish/taffish).
That repository is the canonical place for source code, current installers,
supported platforms, release versions, runtime dependencies, container backend
notes, network notes, source-build instructions, release verification, and
troubleshooting.

Quick user install:

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sh -s -- --user
```

System install for shared servers:

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sudo sh -s -- --system
```

For users in China, the Gitee installer downloads from the Gitee mirror and can
initialize the China mirror profile:

```sh
curl -fsSL https://gitee.com/taffish-org/taffish/raw/main/install/install-taffish.gitee.sh | sh -s -- --user
```

China/Gitee system install:

```sh
curl -fsSL https://gitee.com/taffish-org/taffish/raw/main/install/install-taffish.gitee.sh | sudo sh -s -- --system
```

China/Gitee note: the Gitee mirror is provided for environments where GitHub raw
content is slow or blocked. Some mirror services may restrict anonymous raw
downloads of large files, especially for larger macOS binaries. If the Gitee
installer cannot fetch a binary, use the GitHub installer with a working
network/proxy, download manually after logging in to the mirror, or check the
current notes in the [taffish/taffish README](https://github.com/taffish/taffish).

After `taf` is available locally, the usual starting point is:

```sh
taf doctor
taf update
taf search <keyword>
taf install <app>
taf info <app>
taf list
```

For a pinned release install, platform-specific requirements, Gitee/macOS notes,
offline installation, shell completion, Vim syntax files, and detailed
troubleshooting, use the current
[taffish/taffish README](https://github.com/taffish/taffish).

TAFFISH `0.9.0` is the current public release. The source repository includes [Build From Source](https://github.com/taffish/taffish/blob/main/docs/dev/en/build-from-source.md),
[Contributing](https://github.com/taffish/taffish/blob/main/CONTRIBUTING.md),
and [Security Policy](https://github.com/taffish/taffish/blob/main/SECURITY.md)
documents. The `0.9.0` release adds structured backend-specific container
runtime arguments, local backend runtime-argument environment variables,
external binary-level tests, refreshed release payloads, and `SHA256SUMS`,
`SHA256SUMS.asc`, and `TAFFISH-RELEASE-KEY.asc` for manual checksum and
signature verification.

Persistent mirror configuration can be initialized with:

```sh
taf config init --china --force
taf update
```

Runtime config can change the index URL used by `taf update` and rewrite
canonical GitHub app repository URLs during `taf install`, which makes Gitee or
internal Git service mirrors possible without changing the official index
schema.

For installed commands, `TAFFISH_CONTAINER_BACKEND=apptainer|podman|docker` can
force generic container tags at runtime. App-specific runtime needs should live
in the `.taf` container tag with `$@[target: args]`; one-off machine, site, GPU,
or platform policies should use `TAFFISH_DOCKER_RUN_ARGS`,
`TAFFISH_PODMAN_RUN_ARGS`, or `TAFFISH_APPTAINER_RUN_ARGS`. For private or
local apps that are not yet published to the public Hub, use
`taf install --from <PROJECT-DIR>`.

For a guided first run, read the
[TAFFISH Quick Start](https://github.com/taffish/taffish-docs/blob/main/en/quick-start.en.md).

## How TAFFISH Hub Works

TAFFISH Hub is currently GitHub-based. It publishes executable package metadata
for local `taf` commands rather than running user jobs on a server.

1. Each app lives in its own repository with a root `taffish.toml`.
2. App repositories publish release tags such as `v0.1.0-r1`.
3. App repositories build container images in their own GitHub Actions workflows.
4. [taffish-index](https://github.com/taffish/taffish-index) scans the organization, validates new versions, records dependency, platform, container digest, smoke, trust, and upstream metadata, and writes static JSON index files.
5. Users run `taf update`, then install and run apps locally through `taf`.

The public web Hub at [taffish.github.io](https://taffish.github.io) is the
human-facing browser for this ecosystem. The index repository is the
machine-facing data source.

## For Users

Use [TAFFISH Hub](https://taffish.github.io) to browse available executable
packages and use `taf` locally to install and run them. For installation, CLI
behavior, `.taf` syntax, container usage, and troubleshooting, read
[taffish/taffish-docs](https://github.com/taffish/taffish-docs).

## For App Developers

TAFFISH app projects are structured executable-package repositories with
`taffish.toml`, `src/main.taf`, `docs/help.md`, `target/` build artifacts,
versioned release tags, and optional metadata for dependencies, platform
constraints, containers, smoke checks, Hub trust status, and upstream software
sources.

The official Hub is curated by the `taffish` organization. At the moment,
publishing to the official Hub is limited to organization members. Developers
who want to contribute apps can contact the maintainer to discuss joining the
organization or publishing strategy.

## Paper

The TAFFISH project is described in the bioRxiv preprint:

[TAFFISH: A lightweight, modular, and containerized workflow framework for reproducible bioinformatics analyses](https://www.biorxiv.org/content/10.1101/2025.09.15.672424v2)

Authors: Kaiyuan Han, Ting Wang, Shi-Shi Yuan, Cai-Yi Ma, Wei Su, Kejun Deng,
Xiaolong Li, Hao Lv, and Hao Lin.

## Project Status

TAFFISH is open source and under active development. The local CLI/compiler
implementation is published in [taffish/taffish](https://github.com/taffish/taffish)
under Apache License 2.0. The current public infrastructure is built around
GitHub repositories, GitHub Actions, GitHub Packages, GitHub Pages, and a static
package index. A dedicated server-backed Hub may become useful later, but the
current design intentionally keeps the publishing and indexing path simple,
auditable, and easy to reproduce.
