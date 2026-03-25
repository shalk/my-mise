# Java Version Management (mise)

## Choosing a JDK Distribution

mise supports multiple JDK distributions, identified by version prefix:

| Distribution | Prefix Example | Description |
|--------------|---------------|-------------|
| Eclipse Temurin (recommended) | `temurin-21` | Adoptium build, official OpenJDK, LTS support |
| OpenJDK (default) | `21` | Upstream OpenJDK, no prefix needed |
| GraalVM | `graalvm-21` | Supports native-image compilation |
| Corretto | `corretto-21` | Amazon's distribution |
| Zulu | `zulu-21` | Azul's distribution |
| Microsoft | `microsoft-21` | Microsoft's distribution |

List all available versions of a specific distribution:

```bash
mise ls-remote java | grep "^temurin-21"
```

This repo uses **Eclipse Temurin** — see [Adoptium official releases](https://adoptium.net/temurin/releases/).

## Installing Java

```bash
# List all available versions
mise ls-remote java

# Install Temurin 21 (recommended — picks the latest 21.x)
mise install java@temurin-21

# Install other versions
mise install java@temurin-17
mise install java@temurin-11

# List installed versions
mise list java
```

## Switching Java Versions

### Global switch (system default)

```bash
mise use --global java@temurin-21
```

This modifies `~/.config/mise/config.toml` (i.e., this repo's `mise.toml`).

### Project-level switch (recommended)

```bash
cd /path/to/your-project
mise use java@temurin-17
```

Creates a `.mise.toml` in the project directory. Java switches automatically when you enter the directory.

### Temporary switch (current shell only)

```bash
mise shell java@temurin-11
```

Reverts when you exit the shell.

## Verify Current Version

```bash
mise current java    # version activated by mise
java -version        # version in effect
echo $JAVA_HOME      # JAVA_HOME set automatically by mise
mise where java      # installation directory
```

## Uninstall

```bash
mise uninstall java@temurin-11
```

## macOS System Integration (optional)

mise installs Java to `~/.local/share/mise/installs/java/<version>/`, independent of the system path `~/Library/Java/JavaVirtualMachines`. `JAVA_HOME` is managed automatically by mise — no manual setup needed.

To make the JDK discoverable by IDEs or system tools via the standard path, create a symlink manually:

```bash
# Find the mise installation path
mise where java

# Create a symlink (example: Temurin 21)
sudo mkdir /Library/Java/JavaVirtualMachines/temurin-21.jdk
sudo ln -s ~/.local/share/mise/installs/java/temurin-21.0.10+7.0.LTS/Contents \
  /Library/Java/JavaVirtualMachines/temurin-21.jdk/Contents
```
