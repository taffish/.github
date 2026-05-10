# TAFFISH

[English](README.md) | [中文](README.cn.md)

TAFFISH is an open bioinformatics tool and workflow ecosystem for building,
sharing, and running reproducible analyses with versioned apps, containerized
runtime environments, and a lightweight shell-native workflow language.

It sits between ad-hoc shell scripts and heavyweight workflow systems: close
enough to the command line for everyday bioinformatics work, but structured
enough to make tools, environments, parameters, and workflows reusable.

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
| [taffish.com](https://taffish.com) | Official project homepage, project story, history, and public entry point. |
| [TAFFISH Hub](https://taffish.github.io) | Browse available TAFFISH apps, tools, flows, versions, dependencies, and install commands. |
| [taffish/taffish](https://github.com/taffish/taffish) | Binary distribution for the local `taf` and `taffish` commands; canonical installation entry point. |
| [taffish/taffish-docs](https://github.com/taffish/taffish-docs) | Documentation for TAFFISH, TAFFISH Hub, app projects, `.taf` scripts, containers, dependencies, and publishing. |
| [taffish/taffish-index](https://github.com/taffish/taffish-index) | Generated static package index consumed by `taf update`, `taf search`, `taf info`, and `taf install`. |
| [taffish/taffish.github.io](https://github.com/taffish/taffish.github.io) | Source repository for the web Hub. |
| [taffish/.github](https://github.com/taffish/.github) | This organization profile. |

## What TAFFISH Provides

- A small workflow language for composing bioinformatics tools and flows.
- `taf`, a local package manager and runner for TAFFISH apps.
- Versioned app releases using the `version-release` form, such as `0.1.0-r1`.
- Container-aware execution through Apptainer, Podman, or Docker backends.
- Hub indexing so users can discover, update, install, and inspect apps from a static GitHub-hosted index.
- Flow dependency metadata so `taf install` can resolve required app versions automatically.
- `taf publish --release`, backed by ignored `release.md` drafts for publish messages and GitHub Release notes.
- Runtime mirror configuration for China/Gitee or internal Git service mirrors.

## Installation

Install the TAFFISH CLI from [taffish/taffish](https://github.com/taffish/taffish).
That repository is the canonical place for current installers, supported
platforms, runtime dependencies, container backend notes, and troubleshooting.

Quick user install:

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sh -s -- --user
```

System install for shared servers:

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sudo sh -s -- --system
```

Pinned 0.3.0 install:

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sh -s -- --version 0.3.0 --user
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

macOS note: Gitee may require login for anonymous `raw` downloads of large
binary files. If the Gitee installer fails on macOS with a `403` error or a
large-file login message, use the GitHub installer with a working network/proxy,
or log in to Gitee and download the macOS binary manually. The Gitee installer
is currently most useful for Linux amd64 servers, whose binaries are much
smaller and are usually unaffected.

After `taf` is available locally, the usual starting point is:

```sh
taf doctor
taf update
taf search <keyword>
taf install <app>
taf info <app>
taf list
```

Persistent mirror configuration has been supported since TAFFISH `0.2.0`;
the current recommended release is `0.3.0`:

```sh
taf config init --china --force
taf update
```

The runtime config can change the index URL used by `taf update` and rewrite
canonical GitHub app repository URLs during `taf install`, which makes Gitee or
internal Git service mirrors possible without changing the official index
schema.

For a guided first run, read the
[TAFFISH Quick Start](https://github.com/taffish/taffish-docs/blob/main/en/quick-start.en.md).

## How TAFFISH Hub Works

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

Use [TAFFISH Hub](https://taffish.github.io) to browse available apps and use
`taf` locally to install and run them. For installation, CLI behavior, `.taf`
syntax, container usage, and troubleshooting, read
[taffish/taffish-docs](https://github.com/taffish/taffish-docs).

## For App Developers

TAFFISH app projects are structured repositories with `taffish.toml`,
`src/main.taf`, `docs/help.md`, `target/` build artifacts, versioned release
tags, and optional metadata for dependencies, platform constraints, containers,
and upstream software sources.

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

TAFFISH is under active development. The current public infrastructure is built
around GitHub repositories, GitHub Actions, GitHub Packages, GitHub Pages, and a
static package index. A dedicated server-backed Hub may become useful later, but
the current design intentionally keeps the publishing and indexing path simple,
auditable, and easy to reproduce.
