# TAFFISH

[English](README.md) | [中文](README.cn.md) · [Website](https://taffish.com)

**Bring reproducibility back to the shell.**

TAFFISH packages bioinformatics tools and lightweight workflows as versioned
commands. Install a specific version and use it in your existing shell scripts,
HPC jobs, or workflow systems. TAFFISH focuses on command-level reproducibility;
it does not replace your workflow system.

<a id="table-of-contents"></a>
<a id="for-users"></a>

## Start Here

| I want to… | Go to |
| --- | --- |
| Install TAFFISH and run my first app | [Quick Start](https://github.com/taffish/taffish-docs/blob/main/en/quick-start.en.md) |
| Find a tool, version, or installation command | [TAFFISH Hub](https://taffish.github.io/) |
| Explore flows and example reports | [Flow Portal](https://taffish.github.io/flows/) |
| Package a tool of my own | [App Developer Guide](https://github.com/taffish/taffish-docs/blob/main/en/app-developer-guide.en.md) |

<a id="flow-portal-and-examples"></a>

The Flow Portal includes RNA-seq, quality control, BLAST, and phylogeny routes.
For a detailed case study, see the [RNA-seq Flow Family](https://taffish.github.io/rnaseq-flows/).

<a id="what-taffish-provides"></a>
<a id="how-taffish-hub-works"></a>

## How It Fits Together

1. **An app describes a command.** Its `.taf` source describes execution, while
   `taffish.toml` records its identity, version and dependencies. Help travels
   with the app. A tool app wraps a program; a flow combines versioned apps.
2. **The Hub connects releases and resources.** App repositories publish releases
   and build their container images where needed. The [Index](https://github.com/taffish/taffish-index)
   records source identities, image metadata and smoke-check results. Both the
   Hub website and the CLI use this catalog to discover releases.
3. **The installed command runs in your environment.** `taf` installs a selected
   release and creates its versioned command. The `taffish` compiler translates
   TAF source into shell; containerized commands invoke Docker, Podman or
   Apptainer. Your script controls how commands fit into an analysis.

The Hub website does not run your analysis. The local tools live in
[TAFFISH core](https://github.com/taffish/taffish); the browser interface lives in
[the Hub website repository](https://github.com/taffish/taffish.github.io).
See the [system architecture](https://github.com/taffish/taffish/blob/main/docs/architecture/README.en.md)
for the full component map.

<a id="installation"></a>
<a id="try-a-tool"></a>

## Install and Try

On a [supported macOS or Linux system](https://github.com/taffish/taffish#quick-install),
choose **one** installation scope below. You do not need to run both installers.

### For All Users on the Machine

An administrator installs TAFFISH once for a shared workstation or server.
This requires administrator privileges. By default, core commands and shared
app commands go in `/usr/local/bin`; shared data goes in `/opt/taffish`.

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sudo sh -s -- --system
```

After installation, run `export PATH="/usr/local/bin:$PATH"` in your terminal,
then check with `taf --version`. Ensure `/usr/local/bin` is in each user's `PATH`.
Running an installed app does not itself require `sudo`, though the container backend may have its own
permission requirements.

### For Your Account Only

Use this on a personal machine or when you do not have administrator access.
No `sudo` is needed. Core commands go in `~/.local/bin`, and your apps and local
Hub data stay under `~/.local/share/taffish`.

```sh
curl -fsSL https://raw.githubusercontent.com/taffish/taffish/main/install/install-taffish.sh | sh -s -- --user
```

After installation, run `export PATH="$HOME/.local/bin:$HOME/.local/share/taffish/bin:$PATH"`,
then check with `taf --version`.

These `export` commands affect the current shell session and its child processes;
they do not save the setting for future terminals. For a personal installation,
add the same personal `export` line to `~/.zshrc` (Zsh) or `~/.bashrc` (interactive
Bash). For a Bash login shell, put it in `~/.bash_profile` instead, or have that
file load `~/.bashrc`. Open a new terminal and check with `taf --version`.
For a shared installation, the administrator should arrange the appropriate
PATH setup for users. The installer does not edit shell profiles.
No separate Lisp installation is needed. On Linux,
the installer asks you to read and accept the bundled runtime terms.

### China / Gitee

If GitHub downloads are slow or unavailable, use the
[Gitee installer](https://gitee.com/taffish-org/taffish/raw/main/install/install-taffish.gitee.sh).
Replace only the installer URL in your chosen command above; keep its `--system`
or `--user` option and, for system installation, `sudo`.
See the [Gitee instructions](https://github.com/taffish/taffish/blob/main/README.md#gitee).
Offline installation, pinned releases and verification remain in the
[installation guide](https://github.com/taffish/taffish/blob/main/install/README.md).

### Try FastQC

To try a containerized tool, also have Git, network access and a working Docker,
Podman or Apptainer backend. With either core installation above, the following
installs FastQC **for your account only** and prints its version, without input data.

If your personal app command directory is not on `PATH`, first run
`export PATH="$HOME/.local/share/taffish/bin:$PATH"`.

```sh
taf update --user
taf install --user fastqc 0.12.1-r1
taf-fastqc-v0.12.1-r1 fastqc --version
```

Installing the core system-wide does not change later `taf` commands to system
scope. To share this app, an administrator instead uses `sudo taf update --system`
and `sudo taf install --system fastqc 0.12.1-r1`; users then run the installed
command normally. See the [shared-installation guide](https://github.com/taffish/taffish-docs/blob/main/en/system-administration.en.md)
for backend and cache permissions.

The last command runs FastQC inside its container and prints `FastQC v0.12.1`.
Its command name fixes the tool version (`0.12.1`) and packaging revision
(`r1`). The first run may download the container image.

For a real analysis, work in a directory containing your own `reads.fq.gz`.
You can inspect the command before running it:

```sh
taf-fastqc-v0.12.1-r1 --compile fastqc reads.fq.gz
```

`--compile`, placed immediately after the wrapper name, prints the generated
shell, including the container image, mounts and tool arguments. It does not run
FastQC or download its image. To perform the analysis and write the FastQC
reports beside the input file, omit `--compile`:

```sh
taf-fastqc-v0.12.1-r1 fastqc reads.fq.gz
```

The same versioned command can be used in a shell script or a workflow system.
Compiling lets you review execution; it does not validate your data or results.

## Capabilities and Guides

| Capability | What it provides | Learn more |
| --- | --- | --- |
| App management | Discover, install and upgrade tools; keep explicit versions for scripts. | [Using the Hub](https://github.com/taffish/taffish-docs/blob/main/en/taffish-hub.en.md) |
| TAF scripting | Describe commands and combine them in `.taf` source, compiled to shell. | [TAF tutorial](https://github.com/taffish/taffish-docs/blob/main/en/taf-script-tutorial.en.md) |
| Container backends | Run containerized apps with Docker, Podman or Apptainer, subject to each app's support. | [Backend setup](https://github.com/taffish/taffish-docs/blob/main/en/quick-start.en.md#container-backends) |
| Flows and reports | Compose versioned apps into reusable analyses, with reports where the flow provides them. A flow needs its own container only if its implementation requires one. | [Flow guide](https://github.com/taffish/taffish-docs/blob/main/en/taf-flow-developer-guide.en.md) |
| Interactive apps | Some apps provide browser interfaces or desktop sessions; their help describes backend, port and access requirements. | [Example: Cytoscape](https://github.com/taffish/cytoscape/blob/v3.10.5-r1/docs/help.md) |
| AI-client integration | `taffish-mcp` exposes inspection, validation and compilation through MCP; it does not execute analyses or start containers. | [MCP guide](https://github.com/taffish/taffish-docs/blob/main/en/taffish-mcp.en.md) |
| Verification information | Inspect recorded source commits, image digests and backend smoke results. These checks are not scientific validation. | [Security and trust model](https://github.com/taffish/taffish-docs/blob/main/en/security-model.en.md) |
| Deployment and diagnosis | Configure personal or shared installations, mirrors and caches; inspect local problems with `taf doctor`. | [Administration](https://github.com/taffish/taffish-docs/blob/main/en/system-administration.en.md) · [Troubleshooting](https://github.com/taffish/taffish-docs/blob/main/en/troubleshooting.en.md) |

The [documentation index](https://github.com/taffish/taffish-docs) links the full
user and developer guides. For version-specific changes, see the
[core releases](https://github.com/taffish/taffish/releases).

<a id="for-app-developers"></a>

## Develop and Contribute

Use the [App Developer Guide](https://github.com/taffish/taffish-docs/blob/main/en/app-developer-guide.en.md)
to package a tool, or the [Flow Developer Guide](https://github.com/taffish/taffish-docs/blob/main/en/taf-flow-developer-guide.en.md)
to combine commands into a flow. You can develop and use apps without publishing
them to the official Hub.

Official Hub publication is currently managed by organization members.
To propose an app or discuss contributing, contact [support@taffish.com](mailto:support@taffish.com).
Report software issues in the relevant repository; core issues go to
[taffish/taffish](https://github.com/taffish/taffish/issues).

<a id="paper"></a>
<a id="project-status"></a>

## Paper and Open Source

Read the bioRxiv preprint:
[TAFFISH: shell-native command-level reproducibility for bioinformatics](https://www.biorxiv.org/content/10.1101/2025.09.15.672424).

Authors: Kaiyuan Han, Ting Wang, Shishi Yuan, Caiyi Ma, Wei Su, Xiaolong Li,
Kejun Deng, Hao Lin, and Hao Lyu.

The core implementation is open source under [Apache License 2.0](https://github.com/taffish/taffish/blob/main/LICENSE).
Packaged third-party tools retain their own licenses.
TAFFISH stands for **Tools And Flows Framework Intensify SHell**.
