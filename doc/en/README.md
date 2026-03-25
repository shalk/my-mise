# my-mise

Manage AI CLI tools and development runtimes globally with [mise](https://mise.jdx.dev/).

## Managed Tools

| Tool | Backend | Description |
|------|---------|-------------|
| claude | aqua (anthropics/claude-code) | Anthropic Claude Code CLI |
| codex | aqua (openai/codex) | OpenAI Codex CLI |
| gemini | npm (@google/gemini-cli) | Google Gemini CLI |
| crush | aqua (charmbracelet/crush) | Charm AI Crush CLI |
| copilot | npm (@github/copilot) | GitHub Copilot CLI |

## Usage

```bash
# Symlink to global config
ln -sf ~/code/github.com/shalk/my-mise/mise.toml ~/.config/mise/config.toml

# Install all tools
mise install

# List installed versions
mise ls

# Check for updates
mise outdated

# Upgrade all tools
mise upgrade

# Interactive upgrade
mise up -i
```

## Documentation

- [mise Tutorial](tutorial.md) — 10-chapter comprehensive guide
- [Python Version Management](python.md)
- [Java Version Management](java.md)
- [Node.js Version Management](node.md)
- [Rust Version Management](rust.md)
- [Go Version Management](go.md)
