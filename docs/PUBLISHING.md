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
   git add <本次修改的文件> && git commit -m "新增 skill: <显示名>" && git push
   ```
   推送后 GitHub 渠道即刻生效：`npx skills add whobot-ai/skills@<skill-slug> -g -y`

4. **发布到 ClawHub**：
   ```bash
   clawhub skill publish ./skills/<skill-slug> \
     --owner <发布者> --slug <skill-slug> --name "<显示名>" \
     --topics "逗号,分隔,最多5个" \
     --source-repo whobot-ai/skills --source-commit $(git rev-parse HEAD)
   ```
   发布前查询已有版本与本手册的 pending 记录，显式选择未占用的版本号。
   提交后检查 `clawhub inspect @<发布者>/<skill-slug> --json`；等待时间由平台决定，
   不把提交回执、基础检查通过或旧版 latest 误当作新版已上线。

## 更新已有 skill

电话 skill 按下方“长期更新约定”执行；其他 skill 按用户指定的仓库和发布范围更新。

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
| ModelScope 组织「呼波特」 | https://modelscope.cn/organization/WhoBot （英文名 WhoBot，2026-10-31 审核通过） |
| ModelScope 电话 skill | https://modelscope.cn/skills/WhoBot/whobot-phonecall-skill |
| 电话 skill 文档站 | https://whobot-com.github.io/openclaw-whobot-skill/ （源码在专用仓库 docs/） |

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

### 提交后的核查

可使用官方 CLI 下载两份 2.0.1 的服务器检查报告：

```bash
npx --yes clawhub@latest scan download @whobot-ai/whobot-phonecall-skill --version 2.0.1
npx --yes clawhub@latest scan download @whobot/whobot-phonecall-skill --version 2.0.1
```

已核查：两份 manifest 均列出 2.0.1、11 个文件及各自名称；基础静态检查为 clean，ClawScan 等其他扫描结果为空。
报告汇总中的 `status: succeeded` 不能当作已公开上线的证明。核查时公开 latest 仍分别为 1.0.0 / 2.0.0。
尚未验证普通发布者后台存在单独的“提交记录”页面，不应向使用者承诺该入口。

## 长期更新约定

用户要求“按之前流程更新发布”时，一次完成输入校验、双仓库同步、安装包重建、提交推送、ClawHub 双发布与证据交付。仅要求同步 GitHub 或查询时不扩大范围。

1. 以用户指定新版文件为准；安全解压，过滤 macOS 元数据。多个候选不擅自选取。
2. 同步纯 skill 到合集子目录和专用仓库根目录，保留专用仓库的独立 MCP、旧知识库和仓库文档；删除上游已移除的镜像文件时，依据旧清单，不清空仓库根目录。
3. 重建 `.skill` 包并比对完整文件集合与内容；检查本地引用、JSON、版本和安装说明。文件内版本和 CLI 协议按源码保留，ClawHub 发布版本单独管理。
4. 保留 gh 多账号配置，显式使用 whobot-ai；提交范围仅包含本次改动。GitHub 和 ClawHub 分别验证登录与权限，已有授权可用时不重复索要确认。
5. 检查两个 owner 的原条目以及 pending 记录，先 dry-run，再发布到 whobot-ai 和 whobot；保存真实 JSON 回执。未知结果先查询，不重复上传，不不断增加版本号规避等待。
6. 查询两个 owner 的新版状态，并下载各自官方报告。pending 可合理间隔等待约 5 分钟；仍未上线则如实交付已提交、尚未上线及证据，不承诺后台持续监控。
7. 回执、官方 ZIP 和可读 manifest 保存在本机下载目录。更新本手册的发布记录并推送；最终直接提供版本、原条目链接、GitHub 提交及回执/报告链接，不让用户另找未经验证的后台入口。

登录授权确实失效时才请用户介入。CLI 设备登录进程必须保持等待；用户完成网页授权后验证本机 whoami，避免将网页已授权误认为本机已登录。浏览器当前账号与 gh / ClawHub CLI 账号可能不同。

## skills.sh 渠道（纳入长期更新流程）

原条目：https://www.skills.sh/whobot-ai/skills/whobot-phonecall-skill 。
官方说明：https://www.skills.sh/docs/faq 、https://www.skills.sh/docs/cli 。
该渠道以 GitHub 为源码，通过官方 CLI 安装遥测收录，不使用 ClawHub 的发布版本与审核流程。

GitHub 更新后，在临时项目目录做一次真实安装验证，避免覆盖用户全局环境：

```bash
npx --yes skills@latest add https://github.com/whobot-ai/skills --skill whobot-phonecall-skill --agent codex --copy --yes --json
```

比对安装文件的完整集合及字节内容，然后检查 skills.sh 原页面是否出现新版特征。不要重复安装刷统计或编造强制刷新接口。安装已最新和页面已刷新必须分别报告。

2026-10-27 核查：官方 CLI 安装成功，11 个文件与最新版逐字节一致；返回 hash 为 `89014355f52fd35ae18305a1c16a98b66ce919898dcfab5b4caaaabc0a626545`。安装后首次检查页面仍显示旧内容，未出现新版“关于呼波特”及品牌入口；不据此宣称页面已刷新。

## ModelScope 渠道（2026-10-31 上线）

条目：https://modelscope.cn/skills/WhoBot/whobot-phonecall-skill ，所有者为 ModelScope 组织 `WhoBot`（中文名「呼波特」），由用户个人账号 zephyr 作为组织成员登录操作。
发布方式：网页「创建 Skills → 从 Github 仓库快速创建」，导入 `https://github.com/whobot-ai/skills` 并只勾选 `skills/whobot-phonecall-skill/SKILL.md`，同步 11 个文件；无需 API Key。
设置：展示名「呼波特AI电话数字员工」、MIT、类型「市场推广」、公开，标签 电话数字员工 / AI外呼 / 语音客服 / voice-agent / whobot，**自动同步开启**。

更新电话 skill 后：合集仓库推送即触发 ModelScope 自动同步；用公开接口核查，不需要登录：

```bash
curl -s https://www.modelscope.cn/openapi/v1/skills/WhoBot/whobot-phonecall-skill
```

详情接口的 id 为 `WhoBot/whobot-phonecall-skill`（不带 `@`）。返回中的 `sync_status` 在上线当天仍为 false，但文件页已有 SKILL.md、release.json、references/；以文件页内容为准，不把 `sync_status` 当作唯一依据。
ModelScope 显示的安装命令：`npx skills add https://modelscope.cn/skills/WhoBot/whobot-phonecall-skill`。
