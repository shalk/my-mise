# Python 版本管理（mise）

## 安装 Python

```bash
# 查看可用版本
mise ls-remote python

# 安装指定版本
mise install python@3.13
mise install python@3.11

# 查看已安装的版本
mise list python
```

## 切换 Python 版本

### 全局切换（系统默认）

```bash
mise use --global python@3.13
```

### 项目级切换（推荐）

```bash
cd /path/to/your-project
mise use python@3.11
```

会在项目目录生成 `.mise.toml`，进入该目录后自动切换。

### 临时切换（当前 shell）

```bash
mise shell python@3.11
```

## 验证当前版本

```bash
mise current python    # mise 激活的版本
python --version       # 实际生效的版本
which python           # 二进制路径
```

## 卸载

```bash
mise uninstall python@3.11
```

## 虚拟环境

mise 管理 Python 版本，虚拟环境建议用 **uv**（已在 `mise.toml` 中管理）：

```bash
# 在项目目录创建虚拟环境
uv venv

# 激活
source .venv/bin/activate

# 安装依赖
uv pip install -r requirements.txt
```

也可以用标准 venv：

```bash
python -m venv .venv
source .venv/bin/activate
```

## 安装目录

mise 管理的 Python 安装在 `~/.local/share/mise/installs/python/<version>/`，与系统 Python 完全隔离。
