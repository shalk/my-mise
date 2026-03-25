# Go 版本管理（mise）

## 安装 Go

```bash
# 查看可用版本
mise ls-remote go

# 安装最新版
mise install go@latest

# 安装指定版本
mise install go@1.23
mise install go@1.21

# 查看已安装的版本
mise list go
```

## 切换 Go 版本

### 全局切换（系统默认）

```bash
mise use --global go@latest
```

### 项目级切换（推荐）

```bash
cd /path/to/your-project
mise use go@1.21
```

会在项目目录生成 `.mise.toml`，进入该目录后自动切换。

### 临时切换（当前 shell）

```bash
mise shell go@1.21
```

## 验证当前版本

```bash
mise current go    # mise 激活的版本
go version         # 实际生效的版本
mise where go      # 安装目录
```

## 卸载

```bash
mise uninstall go@1.21
```

## 安装目录

mise 管理的 Go 安装在 `~/.local/share/mise/installs/go/<version>/`，`GOROOT` 由 mise 自动设置，无需手动配置。

`GOPATH` 默认仍为 `~/go`，与 mise 无关。
