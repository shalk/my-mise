# Node.js 版本管理（mise）

## 安装 Node.js

```bash
# 查看可用版本
mise ls-remote node

# 安装指定版本（推荐 LTS）
mise install node@22
mise install node@20
mise install node@18

# 查看已安装的版本
mise list node
```

## 切换 Node.js 版本

### 全局切换（系统默认）

```bash
mise use --global node@22
```

### 项目级切换（推荐）

```bash
cd /path/to/your-project
mise use node@18
```

会在项目目录生成 `.mise.toml`，进入该目录后自动切换。

### 临时切换（当前 shell）

```bash
mise shell node@18
```

## 验证当前版本

```bash
mise current node    # mise 激活的版本
node --version       # 实际生效的版本
npm --version        # npm 版本（随 node 一起安装）
```

## 卸载

```bash
mise uninstall node@18
```

## 全局 npm 包

mise 切换 Node 版本后，各版本的全局 npm 包是**隔离的**，切换版本后需重新安装全局包。

```bash
# 查看当前版本的全局包
npm list -g --depth=0

# 安装全局包（安装到当前激活的 node 版本下）
npm install -g <package>
```

> 提示：常用的全局 CLI 工具（如 `gemini`、`copilot`）已通过 `mise.toml` 的 `npm:` 后端管理，无需手动 `npm install -g`。

## 安装目录

mise 管理的 Node.js 安装在 `~/.local/share/mise/installs/node/<version>/`，与系统 Node 完全隔离。
