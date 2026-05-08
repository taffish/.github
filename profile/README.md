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
| [TAFFISH Hub](https://taffish.github.io) | Browse available TAFFISH apps, tools, and flows. |
| [taffish/taffish](https://github.com/taffish/taffish) | Core CLI, compiler, language implementation, and installation entry point. |
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

## Installation

Install the TAFFISH CLI from [taffish/taffish](https://github.com/taffish/taffish).
That repository is the canonical place for current installers, platform notes,
and backend configuration.

After `taf` is available locally, the usual starting point is:

```sh
taf update
taf install <app>
taf info <app>
taf list --online
```

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
`taf` locally to install and run them. For language usage, CLI behavior, and
development guides, read [taffish/taffish-docs](https://github.com/taffish/taffish-docs).

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
Xiaolong Li, Hao Lv, and Hao Lin.

## Project Status

TAFFISH is under active development. The current public infrastructure is built
around GitHub repositories, GitHub Actions, GitHub Packages, GitHub Pages, and a
static package index. A dedicated server-backed Hub may become useful later, but
the current design intentionally keeps the publishing and indexing path simple,
auditable, and easy to reproduce.
