# WhoBot AI Skills

WhoBot AI 官方 Agent Skills 仓库。所有 skill 均为**纯文本方案**
（SKILL.md + 参考资料），无脚本依赖，适配 Claude Code、OpenClaw、WorkBuddy、
Cursor、Cline 等 17+ 种 Agent 工具。

## Skill 列表

| Skill | 说明 | 安装 |
|---|---|---|
| [clean-commits](skills/clean-commits) | 生成干净、跟随仓库既有风格的 commit message，零 AI 水印 | `npx skills add whobot-ai/skills@clean-commits -g -y` |

## 安装方式

**通用（推荐，适配 17+ 种 Agent 工具）：**

```bash
npx skills add whobot-ai/skills@<skill-name> -g -y
```

**Claude Code（插件市场）：**

```
/plugin marketplace add whobot-ai/skills
/plugin install whobot-skills@whobot-skills
```

**Claude Code 手动安装：**

```bash
git clone https://github.com/whobot-ai/skills && cp -r skills/skills/<skill-name> ~/.claude/skills/
```

**OpenClaw（ClawHub）：**

```bash
clawhub install <skill-name>
```

**WorkBuddy：** 直接对它说「帮我安装 whobot-ai/skills@<skill-name>」，
或手动复制到 `~/.workbuddy/skills/`。

## 新增 Skill

见 [docs/PUBLISHING.md](docs/PUBLISHING.md)（内部发布手册）与
[docs/skill-template/](docs/skill-template/)（模板）。

## License

MIT © WhoBot AI
