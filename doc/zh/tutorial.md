# mise 教程

mise 实用指南 —— 多语言工具版本管理器。

---

## 1. 什么是 mise？

mise（发音 "meez"）是一个统一的命令行工具，可替代 nvm、pyenv、rbenv、asdf 等。通过一个 TOML 配置文件管理工具版本、环境变量和项目任务。

**核心特性：**

- 一个工具管理一切：node、python、go、rust 及 1000+ 更多
- TOML 配置，可纳入版本控制
- 多后端支持：core、aqua、npm、cargo、pipx、go...
- 支持项目级和全局配置

---

## 2. 安装

```bash
# macOS / Linux
curl https://mise.run | sh

# Homebrew
brew install mise

# Windows (scoop)
scoop install mise
```

在 shell 中激活 mise：

```bash
# bash
echo 'eval "$(mise activate bash)"' >> ~/.bashrc

# zsh
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc

# fish
echo 'mise activate fish | source' >> ~/.config/fish/config.fish
```

重启 shell 后验证：

```bash
mise --version
```

---

## 3. 快速上手

```bash
# 在项目中添加 node 22
mise use node@22

# 这会创建 mise.toml：
# [tools]
# node = "22"

# 安装工具
mise install

# 验证
node --version   # v22.x.x

# 全局添加（到处可用）
mise use -g python@3.12
```

---

## 4. 核心概念

**工具**：mise 能安装的任何东西 —— `node`、`python`、`claude`、`ripgrep` 等。

**后端**：工具的来源 —— `core`、`aqua`、`npm`、`cargo` 等。

**版本**：可以是精确版本 (`20.16.0`)、范围 (`22`) 或 `latest`。

**配置优先级**（高 → 低）：

```
./mise.toml              # 项目级
./mise.local.toml        # 项目本地（不提交）
~/.config/mise/config.toml  # 全局
```

---

## 5. 配置详解

典型的 `mise.toml`：

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

### 版本格式

```toml
[tools]
node = "22"          # 最新的 22.x.x
node = "22.11.0"     # 精确版本
node = "latest"      # 最新稳定版
node = "lts"         # 最新 LTS（node 专用）
```

### 工具选项

```toml
[tools]
# 显式指定后端
"aqua:BurntSushi/ripgrep" = "latest"

# 工具特有选项
python = { version = "3.12", virtualenv = ".venv" }
```

---

## 6. 后端系统

```bash
# 列出所有可用后端
mise backends ls

# 在注册表中搜索工具
mise registry | grep ripgrep
```

| 后端 | 说明 | 示例 |
|------|------|------|
| core | 内置（node、python、go、java...） | `node = "22"` |
| aqua | 预编译二进制 | `"aqua:cli/cli" = "latest"` |
| npm | Node 包 | `"npm:prettier" = "3"` |
| cargo | Rust 包 | `"cargo:ripgrep" = "latest"` |
| pipx | Python 包 | `"pipx:black" = "latest"` |
| go | Go 模块 | `"go:golang.org/x/tools/gopls" = "latest"` |

**如何选择**：如果 `mise registry` 中有该工具，直接用短名即可，mise 会自动选最佳后端。仅在需要时显式指定后端前缀。

---

## 7. 日常命令

```bash
# 安装 mise.toml 中的所有工具
mise install

# 添加工具到配置并安装
mise use node@22

# 列出已安装的工具
mise ls

# 检查更新
mise outdated

# 升级所有工具
mise upgrade

# 交互式升级
mise up -i

# 用指定版本运行命令
mise exec node@20 -- node -e "console.log(process.version)"

# 信任配置文件
mise trust

# 清理未使用的版本
mise prune
```

---

## 8. 任务系统

在 mise.toml 中定义项目脚本，替代 Makefile 或 npm scripts。

```toml
[tasks]
dev = "npm run dev"
build = "npm run build"
lint = "eslint src/"
test = "pytest tests/"

[tasks.deploy]
description = "构建并部署"
depends = ["build"]
run = "rsync -az dist/ server:/app/"
```

```bash
# 运行任务
mise run dev

# 列出任务
mise tasks ls

# 简写
mise run build
```

---

## 9. 环境变量管理

mise 可以按目录管理环境变量，替代 direnv。

```toml
[env]
NODE_ENV = "development"
API_KEY = "dev-key-123"

# 从 .env 文件加载
_.file = ".env"

# PATH 操作
_.path = ["./node_modules/.bin", "./scripts"]
```

当你 `cd` 进入项目目录时，这些环境变量自动生效。离开时自动移除。

---

## 10. 技巧与最佳实践

### 锁定版本

```toml
# mise.toml
[settings]
lockfile = true
```

会生成 `mise.lock` 包含精确版本和校验和。提交它以确保可复现构建。

### CI/CD

```yaml
# GitHub Actions
- uses: jdx/mise-action@v2
```

```bash
# 通用 CI
curl https://mise.run | sh
mise install
```

### 实用设置

```bash
# 查看 mise 在做什么
mise settings set verbose true

# 进入目录时自动安装工具
mise settings set auto_install true
```

### 常见问题

```bash
# 检查当前工具解析
mise doctor

# 查看哪些配置文件生效
mise config ls

# 调试工具解析
mise where node
```
