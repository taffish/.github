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
- [Flow Portal And Examples](#flow-portal-and-examples)
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
| [TAFFISH Flow Portal](https://taffish.github.io/flows/) | Curated entry point for official TAFFISH flow families, route pages, examples, and human-facing flow documentation. |
| [taffish/taffish](https://github.com/taffish/taffish) | Open-source TAFFISH source repository, installers, source-tree developer docs, and binary release payloads for `taf`, `taffish`, and `taffish-mcp`. |
| [taffish/taffish-docs](https://github.com/taffish/taffish-docs) | Documentation for TAFFISH, TAFFISH Hub, app projects, `.taf` scripts, containers, dependencies, and publishing. |

## Flow Portal And Examples

The [TAFFISH Flow Portal](https://taffish.github.io/flows/) is the human-facing
entry point for official flow families. It maps problem-oriented routes built
from installable TAFFISH command packages, while the underlying execution still
comes from ordinary `taf-*` commands and explicit input/output contracts.

The portal links:

- official route families such as RNA-seq, NGS QC, BAM QC, BLAST, and phylogeny;
- flow-specific pages and example reports when a route has a mature public case;
- the deeper [RNA-seq Flow Family](https://taffish.github.io/rnaseq-flows/)
  site, which remains the most complete public flow-family case study.

## What TAFFISH Provides

- Command-level reproducibility for the shell commands bioinformaticians
  already use.
- A `.taf` language and `taffish` compiler for packaging command interfaces as
  shell-native executable apps.
- `taf` and TAFFISH Hub for discovering, installing, inspecting, and running
  versioned tools and lightweight flows.
- Container-aware execution through Docker, Podman, or Apptainer, with metadata
  for versions, dependencies, platforms, smoke checks, and trust signals.
- An open Common Lisp implementation and a conservative `taffish-mcp` interface
  for AI-assisted project inspection.

For detailed CLI behavior, `.taf` syntax, container runtime options, mirrors,
MCP setup, security details, source builds, and release-specific changes, use
[taffish/taffish-docs](https://github.com/taffish/taffish-docs) and the
[taffish/taffish README](https://github.com/taffish/taffish).

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
taf outdated
taf upgrade
taf list
```

For pinned releases, platform-specific requirements, China/Gitee notes, mirror
configuration, source builds, release verification, container backend behavior,
private/local app installs, and troubleshooting, use the current
[taffish/taffish README](https://github.com/taffish/taffish) and
[TAFFISH documentation](https://github.com/taffish/taffish-docs).

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

[TAFFISH: shell-native command-level reproducibility for bioinformatics](https://www.biorxiv.org/content/10.1101/2025.09.15.672424v3)

Authors: Kaiyuan Han, Ting Wang, Shi-Shi Yuan, Cai-Yi Ma, Wei Su, Xiaolong Li,
Kejun Deng, Hao Lin, and Hao Lyu.

## Project Status

TAFFISH is open source and under active development. The local CLI/compiler
implementation is published in [taffish/taffish](https://github.com/taffish/taffish)
under Apache License 2.0. The current public infrastructure is built around
GitHub repositories, GitHub Actions, GitHub Packages, GitHub Pages, and a static
package index. A dedicated server-backed Hub may become useful later, but the
current design intentionally keeps the publishing and indexing path simple,
auditable, and easy to reproduce.
