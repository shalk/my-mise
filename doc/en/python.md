# Python Version Management (mise)

## Installing Python

```bash
# List available versions
mise ls-remote python

# Install a specific version
mise install python@3.13
mise install python@3.11

# List installed versions
mise list python
```

## Switching Python Versions

### Global switch (system default)

```bash
mise use --global python@3.13
```

### Project-level switch (recommended)

```bash
cd /path/to/your-project
mise use python@3.11
```

This creates a `.mise.toml` in the project directory. Python switches automatically when you enter the directory.

### Temporary switch (current shell only)

```bash
mise shell python@3.11
```

## Verify Current Version

```bash
mise current python    # version activated by mise
python --version       # version in effect
which python           # binary path
```

## Uninstall

```bash
mise uninstall python@3.11
```

## Virtual Environments

mise manages Python versions. For virtual environments, use **uv** (already managed in `mise.toml`):

```bash
# Create a virtual environment in the project directory
uv venv

# Activate
source .venv/bin/activate

# Install dependencies
uv pip install -r requirements.txt
```

Or use the standard venv:

```bash
python -m venv .venv
source .venv/bin/activate
```

## Installation Directory

mise installs Python to `~/.local/share/mise/installs/python/<version>/`, fully isolated from the system Python.
