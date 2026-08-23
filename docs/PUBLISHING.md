# Skill 发布手册（内部）

WhoBot AI skills 的标准发布流程。前提：本机 gh 已登录 whobot-ai、
clawhub CLI 已登录 @whobot-ai。

## 新增一个 skill

1. **建目录**：复制模板
   ```bash
   cp -r docs/skill-template skills/<skill-slug>
   ```
   - `<skill-slug>`：小写、连字符，全局唯一（ClawHub 全站唯一）
   - 写好 `SKILL.md` frontmatter（`name` = slug，`description` 写清触发场景）
   - 参考资料放 `references/`，**只放纯文本**，不放脚本/数据库/密钥

2. **README 登记**：在根 README「Skill 列表」表格加一行

3. **提交推送**：
   ```bash
   git add -A && git commit -m "新增 skill: <显示名>" && git push
   ```
   推送后 GitHub 渠道即刻生效：`npx skills add whobot-ai/skills@<skill-slug> -g -y`

4. **发布到 ClawHub**：
   ```bash
   clawhub skill publish ./skills/<skill-slug> \
     --slug <skill-slug> --name "<显示名>" \
     --topics "逗号,分隔,最多5个" \
     --source-repo whobot-ai/skills --source-commit $(git rev-parse HEAD)
   ```
   首次发布自动 1.0.0，之后自动递增；提交后等自动安全扫描（纯文本一般几分钟通过），
   `clawhub inspect <skill-slug>` 查看状态。

## 更新已有 skill

改文件 → commit + push → 重跑第 4 步的 publish 命令（带新 commit sha）即可。

## 红线

- 不放可执行脚本、二进制、数据库文件、API 密钥
- 涉及第三方版权内容（游戏数据、文档摘录等）走文本转述并注明来源
- 公司 skill 一律用 @whobot-ai 身份发布，不用个人账号
