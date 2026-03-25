# Node.js Version Management (mise)

## Installing Node.js

```bash
# List available versions
mise ls-remote node

# Install a specific version (LTS recommended)
mise install node@22
mise install node@20
mise install node@18

# List installed versions
mise list node
```

## Switching Node.js Versions

### Global switch (system default)

```bash
mise use --global node@22
```

### Project-level switch (recommended)

```bash
cd /path/to/your-project
mise use node@18
```

Creates a `.mise.toml` in the project directory. Node switches automatically when you enter the directory.

### Temporary switch (current shell only)

```bash
mise shell node@18
```

## Verify Current Version

```bash
mise current node    # version activated by mise
node --version       # version in effect
npm --version        # npm version (bundled with node)
```

## Uninstall

```bash
mise uninstall node@18
```

## Global npm Packages

Global npm packages are **isolated per Node version**. After switching versions, you need to reinstall global packages for that version.

```bash
# List global packages for the current version
npm list -g --depth=0

# Install a global package (installs under the currently active Node version)
npm install -g <package>
```

> Tip: Common CLI tools like `gemini` and `copilot` are already managed via the `npm:` backend in `mise.toml` — no manual `npm install -g` needed.

## Installation Directory

mise installs Node.js to `~/.local/share/mise/installs/node/<version>/`, fully isolated from the system Node.
