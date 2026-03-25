# Go Version Management (mise)

## Installing Go

```bash
# List available versions
mise ls-remote go

# Install the latest version
mise install go@latest

# Install a specific version
mise install go@1.23
mise install go@1.21

# List installed versions
mise list go
```

## Switching Go Versions

### Global switch (system default)

```bash
mise use --global go@latest
```

### Project-level switch (recommended)

```bash
cd /path/to/your-project
mise use go@1.21
```

Creates a `.mise.toml` in the project directory. Go switches automatically when you enter the directory.

### Temporary switch (current shell only)

```bash
mise shell go@1.21
```

## Verify Current Version

```bash
mise current go    # version activated by mise
go version         # version in effect
mise where go      # installation directory
```

## Uninstall

```bash
mise uninstall go@1.21
```

## Installation Directory

mise installs Go to `~/.local/share/mise/installs/go/<version>/`. `GOROOT` is set automatically by mise — no manual configuration needed.

`GOPATH` defaults to `~/go` as usual and is unaffected by mise.
