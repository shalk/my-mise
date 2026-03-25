# Rust 版本管理（mise）

## 安装 Rust

```bash
# 查看可用版本
mise ls-remote rust

# 安装稳定版（推荐）
mise install rust@stable

# 安装指定版本
mise install rust@1.85
mise install rust@nightly

# 查看已安装的版本
mise list rust
```

## 切换 Rust 版本

### 全局切换（系统默认）

```bash
mise use --global rust@stable
```

### 项目级切换（推荐）

```bash
cd /path/to/your-project
mise use rust@1.82
```

会在项目目录生成 `.mise.toml`，进入该目录后自动切换。

### 临时切换（当前 shell）

```bash
mise shell rust@nightly
```

## 验证当前版本

```bash
mise current rust    # mise 激活的版本
rustc --version      # 编译器版本
cargo --version      # 包管理器版本
```

## 卸载

```bash
mise uninstall rust@1.82
```

## 与 rustup / cargo 的关系

mise 的 Rust 后端**基于 rustup 实现**，安装链为：

```
mise → rustup → rustc / cargo / rust-analyzer / ...
```

`cargo` 不是独立工具，它随 rustc 一起由 rustup 管理，版本始终一致。`rustc`、`cargo` 等二进制实际存放在 `~/.cargo/bin/`，都是指向 rustup 的软链接。

## 安装目录

mise 的 rust 安装在 `~/.local/share/mise/installs/rust/<version>/`，内部是一个 rustup 可执行文件加上指向它的软链接。工具链实际由 rustup 存放在 `~/.rustup/toolchains/`。
