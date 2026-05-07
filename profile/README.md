# TAFFISH

TAFFISH is an open bioinformatics app and flow ecosystem built around versioned, reproducible command delivery.

## Start Here

- Organization: [github.com/taffish](https://github.com/taffish)
- Package index: [taffish/taffish-index](https://github.com/taffish/taffish-index)
- Core project: [taffish/taffish](https://github.com/taffish/taffish)
- GitHub homepage repo: [taffish/taffish.github.io](https://github.com/taffish/taffish.github.io)

## TAFFISH CLI Quick Start

```bash
taf update
taf install <app-name>
taf list --online
```

## For App Maintainers

- Keep app metadata in `taffish.toml`
- Use `version-release` format like `0.1.0-r1`
- Publish tagged releases so index automation can discover them
- For flow projects, define dependencies in `[dependencies]`

---

## 中文说明

TAFFISH 是一个面向生信工具与流程的开源生态，目标是让安装、升级和复现更稳定。

### 主要入口

- 组织主页：[github.com/taffish](https://github.com/taffish)
- 索引仓库：[taffish/taffish-index](https://github.com/taffish/taffish-index)
- 核心仓库：[taffish/taffish](https://github.com/taffish/taffish)
- GitHub 主页仓库：[taffish/taffish.github.io](https://github.com/taffish/taffish.github.io)

### 常用命令

```bash
taf update
taf install <应用名>
taf list --online
```
