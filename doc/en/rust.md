# Rust Version Management (mise)

## Installing Rust

```bash
# List available versions
mise ls-remote rust

# Install stable (recommended)
mise install rust@stable

# Install a specific version
mise install rust@1.85
mise install rust@nightly

# List installed versions
mise list rust
```

## Switching Rust Versions

### Global switch (system default)

```bash
mise use --global rust@stable
```

### Project-level switch (recommended)

```bash
cd /path/to/your-project
mise use rust@1.82
```

Creates a `.mise.toml` in the project directory. Rust switches automatically when you enter the directory.

### Temporary switch (current shell only)

```bash
mise shell rust@nightly
```

## Verify Current Version

```bash
mise current rust    # version activated by mise
rustc --version      # compiler version
cargo --version      # package manager version
```

## Uninstall

```bash
mise uninstall rust@1.82
```

## Relationship with rustup and cargo

mise's Rust backend is **built on top of rustup**. The install chain is:

```
mise → rustup → rustc / cargo / rust-analyzer / ...
```

`cargo` is not a standalone tool — it is managed alongside `rustc` by rustup, so the versions always match. The `rustc`, `cargo`, and other binaries live in `~/.cargo/bin/` as symlinks pointing to rustup.

## Installation Directory

mise installs Rust to `~/.local/share/mise/installs/rust/<version>/`, which contains a rustup executable and symlinks pointing to it. The actual toolchains are stored by rustup in `~/.rustup/toolchains/`.
