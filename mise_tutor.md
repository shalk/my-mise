# mise Tutorial / mise 教程

A practical guide to mise — the polyglot tool version manager.

mise 实用指南 —— 多语言工具版本管理器。

---

## 1. What is mise? / 什么是 mise？

mise (pronounced "meez") is a single CLI that replaces nvm, pyenv, rbenv, asdf, and more. It manages tool versions, environment variables, and project tasks — all through a simple TOML config.

mise（发音 "meez"）是一个统一的命令行工具，可替代 nvm、pyenv、rbenv、asdf 等。通过一个 TOML 配置文件管理工具版本、环境变量和项目任务。

**Key features / 核心特性:**

- One tool for everything: node, python, go, rust, and 1000+ more / 一个工具管理一切
- TOML config, version-controllable / TOML 配置，可纳入版本控制
- Multiple backends: core, aqua, npm, cargo, pipx, go... / 多后端支持
- Per-project or global config / 支持项目级和全局配置

---

## 2. Installation / 安装

```bash
# macOS / Linux
curl https://mise.run | sh

# Homebrew
brew install mise

# Windows (scoop)
scoop install mise
```

Activate mise in your shell / 在 shell 中激活 mise:

```bash
# bash
echo 'eval "$(mise activate bash)"' >> ~/.bashrc

# zsh
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc

# fish
echo 'mise activate fish | source' >> ~/.config/fish/config.fish
```

Restart your shell, then verify / 重启 shell 后验证:

```bash
mise --version
```

---

## 3. Quick Start / 快速上手

```bash
# Add node 22 to your project / 在项目中添加 node 22
mise use node@22

# This creates mise.toml: / 这会创建 mise.toml：
# [tools]
# node = "22"

# Install the tool / 安装工具
mise install

# Verify / 验证
node --version   # v22.x.x

# Add globally (available everywhere) / 全局添加（到处可用）
mise use -g python@3.12
```

---

## 4. Core Concepts / 核心概念

**Tool / 工具**: Anything mise can install — `node`, `python`, `claude`, `ripgrep`, etc.

**工具**: mise 能安装的任何东西 —— `node`、`python`、`claude`、`ripgrep` 等。

**Backend / 后端**: Where the tool comes from — `core`, `aqua`, `npm`, `cargo`, etc.

**后端**: 工具的来源 —— `core`、`aqua`、`npm`、`cargo` 等。

**Version / 版本**: Can be exact (`20.16.0`), range (`22`), or `latest`.

**版本**: 可以是精确版本 (`20.16.0`)、范围 (`22`) 或 `latest`。

**Config hierarchy / 配置优先级** (high → low):

```
./mise.toml              # project / 项目级
./mise.local.toml        # project local (gitignored) / 项目本地（不提交）
~/.config/mise/config.toml  # global / 全局
```

---

## 5. Configuration / 配置详解

A typical `mise.toml` / 典型的 `mise.toml`:

```toml
[tools]
node = "22"
python = "3.12"
"npm:prettier" = "latest"

[env]
DATABASE_URL = "postgres://localhost/mydb"

[settings]
experimental = true
```

### Version formats / 版本格式

```toml
[tools]
node = "22"          # latest 22.x.x / 最新的 22.x.x
node = "22.11.0"     # exact / 精确版本
node = "latest"      # latest stable / 最新稳定版
node = "lts"         # latest LTS (node-specific) / 最新 LTS
```

### Tool options / 工具选项

```toml
[tools]
# Specify backend explicitly / 显式指定后端
"aqua:BurntSushi/ripgrep" = "latest"

# Tool-specific options / 工具特有选项
python = { version = "3.12", virtualenv = ".venv" }
```

---

## 6. Backends / 后端系统

```bash
# List all available backends / 列出所有可用后端
mise backends ls

# Search for a tool in the registry / 在注册表中搜索工具
mise registry | grep ripgrep
```

| Backend / 后端 | Description / 说明 | Example / 示例 |
|---|---|---|
| core | Built-in (node, python, go, java...) / 内置 | `node = "22"` |
| aqua | Pre-built binaries / 预编译二进制 | `"aqua:cli/cli" = "latest"` |
| npm | Node packages / Node 包 | `"npm:prettier" = "3"` |
| cargo | Rust crates / Rust 包 | `"cargo:ripgrep" = "latest"` |
| pipx | Python packages / Python 包 | `"pipx:black" = "latest"` |
| go | Go modules / Go 模块 | `"go:golang.org/x/tools/gopls" = "latest"` |

**How to choose / 如何选择**: If `mise registry` lists the tool, just use its short name. mise picks the best backend automatically. Use explicit backend prefix only when needed.

**如何选择**: 如果 `mise registry` 中有该工具，直接用短名即可，mise 会自动选最佳后端。仅在需要时显式指定后端前缀。

---

## 7. Daily Commands / 日常命令

```bash
# Install all tools in mise.toml / 安装 mise.toml 中的所有工具
mise install

# Add a tool to mise.toml and install / 添加工具到配置并安装
mise use node@22

# List installed tools / 列出已安装的工具
mise ls

# Check for updates / 检查更新
mise outdated

# Upgrade all tools / 升级所有工具
mise upgrade

# Upgrade interactively / 交互式升级
mise up -i

# Run a command with a specific tool version / 用指定版本运行命令
mise exec node@20 -- node -e "console.log(process.version)"

# Trust a config file / 信任配置文件
mise trust

# Remove unused tool versions / 清理未使用的版本
mise prune
```

---

## 8. Tasks / 任务系统

Define project scripts in mise.toml, replacing Makefile or npm scripts.

在 mise.toml 中定义项目脚本，替代 Makefile 或 npm scripts。

```toml
[tasks]
dev = "npm run dev"
build = "npm run build"
lint = "eslint src/"
test = "pytest tests/"

[tasks.deploy]
description = "Build and deploy"
depends = ["build"]
run = "rsync -az dist/ server:/app/"
```

```bash
# Run a task / 运行任务
mise run dev

# List tasks / 列出任务
mise tasks ls

# Shorthand / 简写
mise run build
```

---

## 9. Environment Variables / 环境变量管理

mise can manage env vars per directory, replacing direnv.

mise 可以按目录管理环境变量，替代 direnv。

```toml
[env]
NODE_ENV = "development"
API_KEY = "dev-key-123"

# Load from .env file / 从 .env 文件加载
_.file = ".env"

# Path manipulation / PATH 操作
_.path = ["./node_modules/.bin", "./scripts"]
```

When you `cd` into the project directory, these env vars are automatically set. When you leave, they're removed.

当你 `cd` 进入项目目录时，这些环境变量自动生效。离开时自动移除。

---

## 10. Tips & Tricks / 技巧与最佳实践

### Lock versions / 锁定版本

```toml
# mise.toml
[settings]
lockfile = true
```

This generates `mise.lock` with exact versions and checksums. Commit it for reproducible builds.

会生成 `mise.lock` 包含精确版本和校验和。提交它以确保可复现构建。

### CI/CD

```yaml
# GitHub Actions
- uses: jdx/mise-action@v2
```

```bash
# Generic CI / 通用 CI
curl https://mise.run | sh
mise install
```

### Useful settings / 实用设置

```bash
# Always see what mise is doing / 查看 mise 在做什么
mise settings set verbose true

# Auto-install tools when entering a directory / 进入目录时自动安装工具
mise settings set auto_install true
```

### Troubleshooting / 常见问题

```bash
# Check current tool resolution / 检查当前工具解析
mise doctor

# See which config files are active / 查看哪些配置文件生效
mise config ls

# Debug tool resolution / 调试工具解析
mise where node
```
