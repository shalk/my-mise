# Java 版本管理（mise）

## 选择 JDK 发行版

mise 支持多种 JDK 发行版，通过版本前缀区分：

| 发行版 | 前缀示例 | 说明 |
|--------|----------|------|
| Eclipse Temurin（推荐） | `temurin-21` | Adoptium 出品，OpenJDK 官方构建，LTS 长期支持 |
| OpenJDK（默认） | `21` | 上游 OpenJDK，无前缀 |
| GraalVM | `graalvm-21` | 支持 native-image 编译 |
| Corretto | `corretto-21` | Amazon 出品 |
| Zulu | `zulu-21` | Azul 出品 |
| Microsoft | `microsoft-21` | 微软出品 |

查看某个发行版的所有可用版本：

```bash
mise ls-remote java | grep "^temurin-21"
```

本仓库使用 **Eclipse Temurin**，对应 [Adoptium 官方发行版](https://adoptium.net/temurin/releases/)。

## 安装 Java

```bash
# 查看所有可用版本
mise ls-remote java

# 安装 Temurin 21（推荐，会自动选最新 21.x）
mise install java@temurin-21

# 安装其他版本
mise install java@temurin-17
mise install java@temurin-11

# 查看已安装的版本
mise list java
```

## 切换 Java 版本

### 全局切换（系统默认）

```bash
mise use --global java@temurin-21
```

修改的是 `~/.config/mise/config.toml`（即本仓库的 `mise.toml`）。

### 项目级切换（推荐）

在项目目录下执行：

```bash
cd /path/to/your-project
mise use java@temurin-17
```

会在项目目录生成 `.mise.toml`，进入该目录后自动切换。

### 临时切换（当前 shell）

```bash
mise shell java@temurin-11
```

退出 shell 后失效。

## 验证当前版本

```bash
mise current java    # mise 激活的版本
java -version        # 实际生效的版本
echo $JAVA_HOME      # mise 自动设置的 JAVA_HOME
mise where java      # 安装目录
```

## 卸载

```bash
mise uninstall java@temurin-11
```

## macOS 系统集成（可选）

mise 的 Java 安装在 `~/.local/share/mise/installs/java/<version>/`，与系统传统路径 `~/Library/Java/JavaVirtualMachines` 相互独立，`JAVA_HOME` 由 mise 自动管理，无需手动设置。

如需让 IDE 或系统工具也能通过标准路径发现，可手动创建软链接：

```bash
# 查看 mise 安装路径
mise where java

# 创建软链接（以 Temurin 21 为例）
sudo mkdir /Library/Java/JavaVirtualMachines/temurin-21.jdk
sudo ln -s ~/.local/share/mise/installs/java/temurin-21.0.10+7.0.LTS/Contents \
  /Library/Java/JavaVirtualMachines/temurin-21.jdk/Contents
```
