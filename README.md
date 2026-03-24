# my-mise

用 [mise](https://mise.jdx.dev/) 全局管理 AI CLI 工具版本。

## 管理的工具

| 工具 | 后端 | 说明 |
|------|------|------|
| claude | aqua (anthropics/claude-code) | Anthropic Claude Code CLI |
| codex | aqua (openai/codex) | OpenAI Codex CLI |
| gemini | npm (@google/gemini-cli) | Google Gemini CLI |
| crush | aqua (charmbracelet/crush) | Charm AI Crush CLI |
| copilot | npm (@github/copilot) | GitHub Copilot CLI |

## 使用方式

```bash
# 软链到全局配置
ln -sf ~/code/github.com/shalk/my-mise/mise.toml ~/.config/mise/config.toml

# 安装所有工具
mise install

# 查看已安装版本
mise ls

# 检查是否有新版本
mise outdated

# 升级所有工具
mise upgrade

# 交互式升级
mise up -i
```
