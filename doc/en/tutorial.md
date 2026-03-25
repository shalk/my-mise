# mise Tutorial

A practical guide to mise — the polyglot tool version manager.

---

## 1. What is mise?

mise (pronounced "meez") is a single CLI that replaces nvm, pyenv, rbenv, asdf, and more. It manages tool versions, environment variables, and project tasks — all through a simple TOML config.

**Key features:**

- One tool for everything: node, python, go, rust, and 1000+ more
- TOML config, version-controllable
- Multiple backends: core, aqua, npm, cargo, pipx, go...
- Per-project or global config

---

## 2. Installation

```bash
# macOS / Linux
curl https://mise.run | sh

# Homebrew
brew install mise

# Windows (scoop)
scoop install mise
```

Activate mise in your shell:

```bash
# bash
echo 'eval "$(mise activate bash)"' >> ~/.bashrc

# zsh
echo 'eval "$(mise activate zsh)"' >> ~/.zshrc

# fish
echo 'mise activate fish | source' >> ~/.config/fish/config.fish
```

Restart your shell, then verify:

```bash
mise --version
```

---

## 3. Quick Start

```bash
# Add node 22 to your project
mise use node@22

# This creates mise.toml:
# [tools]
# node = "22"

# Install the tool
mise install

# Verify
node --version   # v22.x.x

# Add globally (available everywhere)
mise use -g python@3.12
```

---

## 4. Core Concepts

**Tool**: Anything mise can install — `node`, `python`, `claude`, `ripgrep`, etc.

**Backend**: Where the tool comes from — `core`, `aqua`, `npm`, `cargo`, etc.

**Version**: Can be exact (`20.16.0`), range (`22`), or `latest`.

**Config hierarchy** (high → low):

```
./mise.toml              # project
./mise.local.toml        # project local (gitignored)
~/.config/mise/config.toml  # global
```

---

## 5. Configuration

A typical `mise.toml`:

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

### Version formats

```toml
[tools]
node = "22"          # latest 22.x.x
node = "22.11.0"     # exact version
node = "latest"      # latest stable
node = "lts"         # latest LTS (node-specific)
```

### Tool options

```toml
[tools]
# Specify backend explicitly
"aqua:BurntSushi/ripgrep" = "latest"

# Tool-specific options
python = { version = "3.12", virtualenv = ".venv" }
```

---

## 6. Backends

```bash
# List all available backends
mise backends ls

# Search for a tool in the registry
mise registry | grep ripgrep
```

| Backend | Description | Example |
|---------|-------------|---------|
| core | Built-in (node, python, go, java...) | `node = "22"` |
| aqua | Pre-built binaries | `"aqua:cli/cli" = "latest"` |
| npm | Node packages | `"npm:prettier" = "3"` |
| cargo | Rust crates | `"cargo:ripgrep" = "latest"` |
| pipx | Python packages | `"pipx:black" = "latest"` |
| go | Go modules | `"go:golang.org/x/tools/gopls" = "latest"` |

**How to choose**: If `mise registry` lists the tool, just use its short name. mise picks the best backend automatically. Use an explicit backend prefix only when needed.

---

## 7. Daily Commands

```bash
# Install all tools in mise.toml
mise install

# Add a tool to mise.toml and install
mise use node@22

# List installed tools
mise ls

# Check for updates
mise outdated

# Upgrade all tools
mise upgrade

# Upgrade interactively
mise up -i

# Run a command with a specific tool version
mise exec node@20 -- node -e "console.log(process.version)"

# Trust a config file
mise trust

# Remove unused tool versions
mise prune
```

---

## 8. Tasks

Define project scripts in mise.toml, replacing Makefile or npm scripts.

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
# Run a task
mise run dev

# List tasks
mise tasks ls

# Shorthand
mise run build
```

---

## 9. Environment Variables

mise can manage env vars per directory, replacing direnv.

```toml
[env]
NODE_ENV = "development"
API_KEY = "dev-key-123"

# Load from .env file
_.file = ".env"

# Path manipulation
_.path = ["./node_modules/.bin", "./scripts"]
```

When you `cd` into the project directory, these env vars are automatically set. When you leave, they're removed.

---

## 10. Tips & Tricks

### Lock versions

```toml
# mise.toml
[settings]
lockfile = true
```

This generates `mise.lock` with exact versions and checksums. Commit it for reproducible builds.

### CI/CD

```yaml
# GitHub Actions
- uses: jdx/mise-action@v2
```

```bash
# Generic CI
curl https://mise.run | sh
mise install
```

### Useful settings

```bash
# Always see what mise is doing
mise settings set verbose true

# Auto-install tools when entering a directory
mise settings set auto_install true
```

### Troubleshooting

```bash
# Check current tool resolution
mise doctor

# See which config files are active
mise config ls

# Debug tool resolution
mise where node
```
