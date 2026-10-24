# WhoBot AI Skills

WhoBot AI 官方 Agent Skills 仓库。Skill 以纯文本文件提供
（SKILL.md + 参考资料），适配 Claude Code、OpenClaw、WorkBuddy、
Cursor、Cline 等 Agent 工具。部分 skill 会调用官方 CLI，运行要求见对应说明。

## Skill 列表

| Skill | 说明 | 安装 |
|---|---|---|
| [clean-commits](skills/clean-commits) | 生成干净、跟随仓库既有风格的 commit message，零 AI 水印 | `npx skills add whobot-ai/skills@clean-commits -g -y` |
| [whobot-phonecall-skill](skills/whobot-phonecall-skill) | 呼波特 AI 电话数字员工：选型找标杆、体验通话、创建与调优员工、上传线索外呼 | `npx skills add whobot-ai/skills@whobot-phonecall-skill -g -y` |

### 呼波特 AI 电话数字员工

用自然语言帮企业挑选、体验、创建和调优会打电话的 AI 数字员工，适用于回访、邀约、通知、线索筛选、电话销售、热线接听等场景。

- 找标杆：按行业和电话目标推荐合适的数字员工。
- 体验：网页语音对话或真实来电通话。
- 创建与调优：支持多种方式创建、优化自己的数字员工。
- 线索外呼：上传客户名单交给员工拨打，查看进度，暂停或恢复。
- 账号：登录、退出、查询余额。
- 涉及费用或真实拨打的操作，都会先确认再执行。
- 运行依赖 Node.js >= 20（含 npm、npx）和官方 `@whobot/cli`；安装与使用流程见 [SKILL.md](skills/whobot-phonecall-skill/SKILL.md) 及其参考资料。

## 安装方式

**通用（推荐）：**

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

**OpenClaw（ClawHub，适用于已在该渠道发布的 skill）：**

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
