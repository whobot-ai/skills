# Skill 发布手册（内部）

WhoBot AI skills 的标准发布流程。前提：本机 gh 已登录 whobot-ai、
ClawHub CLI 已登录 @whobot-ai。保留本机 gh 多账号配置；操作时显式选择 whobot-ai，
不要登出其他账号或改动全局 Git 凭据。

## 新增一个 skill

1. **建目录**：复制模板
   ```bash
   cp -r docs/skill-template skills/<skill-slug>
   ```
   - `<skill-slug>`：小写、连字符；发布时显式指定 owner，使用 @owner/slug 区分个人与组织版本
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
- 公司 skill 使用 whobot-ai 登录身份；按发布目标显式指定 --owner whobot-ai 或 --owner whobot，不使用 yzfly、LangGPT 等其他账号

## WhoBot 账号与仓库对应关系（2026-10-27 确认）

| 用途 | 地址 / 标识 |
| --- | --- |
| ClawHub 个人发布者 | https://clawhub.ai/whobot-ai |
| ClawHub 组织发布者 | https://clawhub.ai/whobot |
| GitHub 个人账号 | https://github.com/whobot-ai |
| GitHub 组织「WhoBot 呼波特」 | https://github.com/whobot-com |
| 全部 skills 的合集仓库 | https://github.com/whobot-ai/skills |
| 电话 skill 专用仓库（当前） | https://github.com/whobot-com/openclaw-whobot-skill |
| 专用仓库旧地址（已转移） | https://github.com/whobot-ai/openclaw-whobot-skill |
| ClawHub 电话 skill 个人版 | https://clawhub.ai/whobot-ai/whobot-phonecall-skill |
| ClawHub 电话 skill 组织版 | https://clawhub.ai/whobot/whobot-phonecall-skill |
| ClawHub 旧知识库条目 | https://clawhub.ai/whobot-ai/openclaw-whobot-skill |

GitHub 组织 handle 为 `whobot-com`，ClawHub 组织 handle 为 `whobot`，两者不同。
个人版与组织版电话 skill 分别维护在原有条目中，发布时不迁移 owner、不新建替代 slug。
更新电话 skill 时同步合集中的 `skills/whobot-phonecall-skill/`、专用仓库根目录以及专用仓库的 `.skill` 安装包。
ClawHub 发布使用纯文本 skill 目录，不上传旧 MCP 文件、ZIP、`.DS_Store` 或 `__MACOSX`。
发布前分别检查两个 owner 的已有版本，再选择未占用的新版本号；文件内 Skill 协议版本与 ClawHub 发布版本分别记录。

最新版内容来源：`~/Downloads/whobot-phonecall-skill-261027.zip`。
对应代码提交：合集 `1d1fb15`，专用仓库 `32878cd`；文件内版本为 2.0.0，CLI 协议为 2。

## ClawHub 发布记录：2026-10-27

已使用 `whobot-ai` 登录身份分别提交个人版和组织版 `2.0.1`，不迁移所有权。
源代码为 `whobot-ai/skills@1d1fb15bf29d4b150843e7e4681cbc45eb843ac2` 的 `skills/whobot-phonecall-skill`。
两份上传均含 11 个文件，内容指纹均为 `b3c878d192341ed595c618c2ca431ceb1e786a057f1fa9cfe34344cded5bf0d2`。
文件内 Skill 版本仍为 2.0.0；ClawHub 的发布版本为 2.0.1。

| 发布者 | 版本 ID | 提交状态 |
| --- | --- | --- |
| whobot-ai | k977rtpgfxfn4e4x97883b69xx8h20vr | pending-publication，等待平台检查 |
| whobot | k97bkw95dmrymt8hx7hkwd3ba58h25cm | pending-publication，等待平台检查 |

提交成功不代表公开上线；以 `clawhub inspect @<owner>/whobot-phonecall-skill --json` 的最新版本和平台审核状态为准。
