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
| 电话 skill 文档站 | https://www.whobot.com/skills/docs/ （官网版，源码在专用仓库 docs/） |
| 电话 skill 文档站 GitHub 版 | https://whobot-com.github.io/openclaw-whobot-skill/ （同一份源码，canonical 指向官网） |

GitHub 组织 handle 为 `whobot-com`，ClawHub 组织 handle 为 `whobot`，两者不同。
个人版与组织版电话 skill 分别维护在原有条目中，发布时不迁移 owner、不新建替代 slug。
更新电话 skill 时同步合集中的 `skills/whobot-phonecall-skill/`、专用仓库根目录以及专用仓库的 `.skill` 安装包。
ClawHub 发布使用纯文本 skill 目录，不上传旧 MCP 文件、ZIP、`.DS_Store` 或 `__MACOSX`。
发布前分别检查两个 owner 的已有版本，再选择未占用的新版本号；文件内 Skill 协议版本与 ClawHub 发布版本分别记录。

2026-10-27 历史版本来源：`~/Downloads/whobot-phonecall-skill-261027.zip`。
对应代码提交：合集 `1d1fb15`，专用仓库 `32878cd`；当时文件内版本为 2.0.0，CLI 协议为 2。
当前内容基线为合集 `0155683202b5af58158080ade07ec82eaf8e3a81` 中的电话 skill：文件内版本 2.0.7、CLI 协议 2、11 个纯文本文件；2.0.7 已同步到专用仓库（`230f61b`，含安装包与文档），尚未同步到各发布渠道；最近一次渠道同步状态见下方 2.0.6 记录。

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

## 2.0.4 全渠道同步记录（2026-11-02 UTC）

内容基线：`whobot-ai/skills@5f6f70a46aeacdaaf1ee759ca2dcbcaf8c004339`，skill tree `c40fbf13f08a0995e44e11c3a38711d47e15308a`。本版新增 `references/company.md`，更新账号、员工创建、标杆查找、试打、安装和协议说明。

| 渠道 | 已验证状态 |
| --- | --- |
| GitHub 合集 | 2.0.4 内容已在上述源提交中公开。 |
| GitHub 专用仓库 | 已推送 [`c53437e`](https://github.com/whobot-com/openclaw-whobot-skill/commit/c53437e49e345094466c865d7df19ec6535a68c2)；12 个 skill 文件与合集逐字节一致，保留独立 MCP、旧知识库和其他仓库文件。 |
| `.skill` 安装包 | 已重建并推送；压缩包文件集合及内容与 12 个源文件完全一致，SHA256 `211e46ef3e43f5aa0c4d7345571cdb93610991bdbec436a82ee9fa3e60b4a5de`。 |
| 文档站 | docs-check 和 Pages 部署成功；公开 `llms-full.txt` 与新生成文件逐字节一致。 |
| skills.sh | 官方 CLI 1.7.4 在隔离项目中完成一次真实安装，12 文件逐字节一致；原页面已显示 `whobot status --skill 2.0.4` 和新版网页通话升级恢复说明。 |
| SkillHub 官方条目 | [`@org-teo38e1k/whobot-phonecall-skill`](https://skillhub.cn/skills/org-teo38e1k/whobot-phonecall-skill) 已公开为 2.0.4，发布者为北京呼波特人工智能科技有限公司；CLI 安装后全部 references、release.json 和正文与源文件一致。仅 `description_zh` 少一行“仅需网站文字客服、聊天机器人或纯语音合成时不适用”，另附平台 `_meta.json`；未重复提交同版本。 |
| ClawHub 个人 / 组织 | 双 dry-run 通过，但上传入口持续返回 HTTP 400 / `Too many writes in a short period of time`；2.0.4 **尚未提交、未进入 pending**。最终查询两个原条目 latest 仍为 2.0.1，2.0.4 版本查询和官方报告下载均返回 `Skill version not found`。 |
| GitCode | 原仓库确认为 [`whobot/openclaw-whobot-skill`](https://gitcode.com/whobot/openclaw-whobot-skill)，当前 `main` 为 `07c69156a2e5bd8a067d5dac9a309eca8149f76a`、内容 2.0.0。已准备与专用 GitHub 仓库一致的 20 个变更文件；本次 CLI 无凭据，非交互 Git dry-run 也未取得凭据，尚未推送。 |
| ModelScope | 自动同步已启用，但本次核查页面内容仍为 2.0.0，最近同步显示 `2026-11-01 10:06:38`。已打开网页手动同步确认流程，尚未完成确认或验证新版；不得据此记为上线。 |

本次本地证据目录：`~/Downloads/whobot-release-2.0.4-20261101/`。包含安装包、skills.sh 安装回执与页面快照、SkillHub 逐文件比对、ClawHub dry-run/失败回执及脱敏 HTTP 诊断。ClawHub 错误发生在首个文件申请 upload URL 时，尚未上传文件或调用发布 POST；按返回 reset 等待后仍失败，已停止重复写入，没有可交付的新版本扫描 ZIP。

GitCode 尚未登录或浏览器发布未完成时，不把本地准备完成当作已同步。后续继续原仓库、原条目和版本 2.0.4；先查远端与 pending 状态，再恢复发布，不创建替代 slug 或递增版本号规避平台故障。


## 2.0.5 渠道同步记录（2026-11-05，回执 UTC 2026-11-06）

核对源提交：合集 `d87b0efeebbea0cd00f6b6f574a4bfb369ddd523`、专用仓库 `b8430ca3b760428ff374d5f4272f769542336e82`。两仓 12 个纯 Skill 文件及专用仓库 `.skill` 内容一致，CLI 协议保持 2。本次沿用已有条目、资产和仓库，不迁移 owner 或创建替代 slug。

| 渠道 | 已验证状态 |
| --- | --- |
| ClawHub 个人 | [原条目](https://clawhub.ai/whobot-ai/whobot-phonecall-skill) latest 2.0.5，moderation clean；versionId `k97d0vg6fswkpkgth99m8frdys8hq5cm`，attemptId `zx7a3aczsea0g07gwpq0pz6ny98hqpjb`。 |
| ClawHub 组织 | [原条目](https://clawhub.ai/whobot/whobot-phonecall-skill) latest 2.0.5，moderation clean；versionId `k97dz0pb3ham9pv3dkf8dscsvn8hq7pe`，attemptId `zx782wzke8ew2mwsdg82e3yt7x8hq9sa`。 |
| skills.sh | [原页面](https://www.skills.sh/whobot-ai/skills/whobot-phonecall-skill) 已显示 2.0.5；隔离目录完成一次真实安装，12 文件逐字节一致。 |
| ModelScope | [原条目](https://modelscope.cn/skills/WhoBot/whobot-phonecall-skill) 手动同步成功，自动同步继续启用；公开 ZIP 中 12 文件与源文件逐字节一致。 |
| GitCode | [原仓库](https://gitcode.com/whobot/openclaw-whobot-skill) 提交 `811b1ed5def894c41c6b43c1dfff9e7161654628`，保留原历史及独立 MCP。已发布 [v2.0.5](https://gitcode.com/whobot/openclaw-whobot-skill/releases/v2.0.5)；Git fetch 比对 12 文件及 `.skill` 均一致，发行版附件重新下载也与源包一致。 |
| WorkBuddy | 原资产 `os_09a8d4e2459431ea` 提交 2.0.5，审核中；最终上传 ZIP 本地 WorkBuddy 预检 PASS。 |
| SkillHub 个人 | [原条目](https://skillhub.cn/skills/user_fa822256/yzfly-whobot-voice-agent) 提交更新 2.0.5，安全审核中。 |
| 千问办公 | 原申请更新 2.0.5，安全扫描中。页面沿用原申请时间；历史 2.0.4 自测资料已明确标注，不冒充 2.0.5 运行测评。 |
| 豆包 | 原能力 `7703563794026335244` 提交 2.0.5，审核中。description 按平台限制缩短，完整正文和参考资料保留；历史自测材料版本已明确标注。 |
| SkillHub 企业 | [原条目](https://skillhub.cn/skills/org-teo38e1k/whobot-phonecall-skill) 已显示 2.0.5，但安装后 `SKILL.md` 与 `references/create-agent.md` 不同于最新源文件，后者缺少新版草稿恢复指令。当前个人后台不含该企业资产，需所属账号处理；未重复提交同版本或递增版本绕过占用。 |
| 百度 DuMate | 原平台版本 1.0.0（源内容 2.0.4）仍审核中，更新按钮禁用；已准备 2.0.5 包，尚未上传，未撤回原申请。 |

ClawHub 使用 `whobot-ai` 身份双 dry-run 后分别发布；两份内容指纹均为 `afc25ffe1cee4a4a0e0130e6bab0223e18bd3f729d8f8169be0bed525bdd4505`。两份官方扫描 ZIP 已下载，manifest 均为 2.0.5、12 文件、sourceKind published、status succeeded；ClawScan 均为 clean/benign。平台 verdict 与本地预检分开记录。

`.skill` 与 GitCode 发行版附件 SHA-256：`46bab7474f9cb7a71ea1de1da5ae1439ef9da1e549001f0905e1ca4f05c3671d`。
WorkBuddy/SkillHub 适配 ZIP：`c95ae28e58a5a1b849515269ebd2ae8064645cf01e7d179af930da7c978f650f`；千问：`8d0efcc423e74cfd9eb78440480337c21f4ab2d673c85de8c8bf5c5acf42e230`；豆包：`67244d3077848af26e2b8723a879746175528d685de6c2139c5c939ceef71c2d`；百度候选：`42f3c1ca7c4071a6aa75cbfd35e769d2465074aa6b755d8bc68402a34c68e7ed`。

控制机证据目录：`whobot-mcp-app/auto/artifacts/releases/2026-11-05-whobot-2.0.5/`，入口 `publication-results.json`，含最终 ZIP、递归预检、安全扫描、网页回执和逐文件对比。MacAir 下载目录：`~/Downloads/whobot-release-2.0.5-20261105/`，保留 ClawHub 双发布回执与官方报告。审核中不等于已上线；百度候选测试用例未在平台沙箱运行，本次未执行真实电话或收费操作。

## 2.0.6 渠道同步记录（2026-11-08，回执 UTC 2026-11-09）

源基线为合集 `928acc6ff8edf0a1ef13090a81b951150fa18a54`（Skill 变更 `eb637304d5fecdfd86c4b520a59a2041eea0d51f`）及专用仓库 `06bb7b54462eedc87d26b811f7638972cc4e61f8`。11 个纯 Skill 文件和 `.skill` 内容一致，协议 2；`company.md` 合并到 `product.md`。本轮在 MacAir 沿用原条目同步。

| 渠道 | 已验证状态 |
| --- | --- |
| ClawHub 个人 / 组织 | 两个原条目 latest 均 2.0.6，顶层 moderation clean。个人 versionId `k973909ebbggvwsb7j9f81gbgh8hxxsf`；组织 `k9787qe30nqj565zy4q8c6h5fh8hxhws`。11 个源文件哈希均一致；个人公开版本另有 `skill-card.md`。 |
| skills.sh | 隔离目录真实安装成功，11 文件逐字节一致；原页面已显示 2.0.6。 |
| ModelScope | 手动同步成功，公开 ZIP 11 文件逐字节一致，自动同步继续开启。 |
| GitCode | 原仓库提交 `db5edb5c2a7ed6b9e19f3d7f2b25608fe265177a`，保留历史和独立 MCP；[v2.0.6 发行版](https://gitcode.com/whobot/openclaw-whobot-skill/releases/v2.0.6) 已发布，重新下载附件与源 `.skill` 一致。 |
| WorkBuddy | 原资产 `os_09a8d4e2459431ea` 提交 2.0.6，审核中。最终 r1 ZIP 递归本地预检 PASS；初版描述超限仅解析失败，未提交。 |
| 千问企业版 | 2.0.6 安全检查通过，运行测评已退回；平台要求新版六类完整业务录屏，历史 2.0.4 材料不足。等待新版材料，或指定测试账号、员工、号码名单和费用上限后补录，未重复提交。 |
| 千问个人版 | 用户明确“不上传个人版，走企业版”；个人旧申请未修改、未重提。 |
| SkillHub 企业版 | 用户完成企业账号授权后，原条目 `org-teo38e1k/whobot-phonecall-skill` 单次提交 2.0.6，安全审核中，公开版仍 2.0.5；之后需团队管理员审核并核对对外发布。个人条目本轮未更新。 |
| 豆包 | 原申请退回说明为“11.6 暂缓三方纯skill上架，请改造成mcp，组合成plugin以插件形式上架”。2.0.6 候选包已准备，未重复提交或扩建插件。 |
| 百度 DuMate | 原平台 1.0.0 / 源 2.0.4 仍审核中、更新禁用；2.0.6 候选包已准备，未撤回原申请。 |

ClawHub 官方报告已保存：个人版本 security clean / hasWarnings，Skillspector LOW 18；组织版本 security suspicious，Skillspector MEDIUM 26、ClawScan suspicious。主要提示可变版本 CLI 安装和录音本地保存缺少隐私提醒，并声明部分扫描覆盖受限。顶层 moderation clean 不代表版本级扫描没有警告。上传指纹均为 `8fc2d0ede44df4b176988d39133fda28da412e92dd67e0b9ccd99cb8f8b92cc5`。组织额外发布请求被“Version 2.0.6 already exists”拒绝后查询原成功版本，未创建替代版本。

`.skill` / GitCode 附件 SHA-256：`43c7439eaba15be518f695cda46799c238233bfc69d1977bb49ed7f16ddad50d`。
WorkBuddy 最终 r1：`5b58b0a93967d22a9d310ba9cd03bc2b4aed14f19a943fe121a1407e165e2a4c`；千问：`9f7224e46bca91a272fe536f326d66cfebd1fd347b8abb4e3cc9733ed8c627b3`；豆包候选：`654190d30fccb4a4b1e080a674274fbfeccd814338f14182f8a53c6145afb162`；百度候选：`4c4aff494ec96c4d2993104e9ccc30b09df4e85daa440b9181eb89e057a0085d`。

控制机证据：`whobot-mcp-app/auto/artifacts/releases/2026-11-08-whobot-2.0.6/`，入口 `README.md` / `publication-results.json`；MacAir `/Users/jiangshu/whobot-release-2.0.6-20261108`。平台审核与本地预检分开记录；本轮没有真实拨号、创建/调整员工、上传客户名单或收费业务。百度候选测试用例未在平台沙箱运行。用户已完成 SkillHub 企业授权，沿用 Ego Lite TaskSpace 84 续传。

### 企业授权后续传（UTC 2026-11-09 07:50）

SkillHub 登录团队为 `org-teo38e1k`（北京呼波特人工智能科技有限公司），原条目可在「管理后台 → 技能列表 → 更多 → 更新」维护。提交前核对 12 个文件、原 slug、显示名、图标、分类及新版完整描述；仅提交一次。最终 ZIP `whobot-phonecall-skill-2.0.6-skillhub-corporate.zip`，SHA-256 `e9a01a75f67b9691d7a61c1067820fd40fd40b64908cfad337f9f251ddfa0d6c`，递归预检 PASS。平台回执“v2.0.6 已提交审核”，审核页为安全审核中；不把提交回执当作上线。按官方企业流程，自动安全审核之后还有团队管理员审核；不以管理员通过按钮跳过正在进行的安全审核。

千问企业版复查为运行测评失败，内容合规与 Skill 包安全均通过。实际要求补录新版行业查询、语音/电话体验、创建或调优、线索外呼及线索包、账号登录与余额、产品介绍与选型六类完整过程。已请求新版材料位置或测试资源与费用上限；没有擅自执行收费业务。WorkBuddy 2.0.6 复查仍审核中。

新增回执位于原证据目录的 `macair-evidence/skillhub-corporate-*`、`qwen-corporate-latest.txt`、`qwen-corporate-rejection.txt`、`workbuddy-latest.txt`；入口总表和报告已更新。后续先查 2.0.6 当前状态，审核完成后继续同一原条目，勿重提或创建替代版本。


## 2.0.7 渠道同步记录（2026-11-09）

用户提供的两个仓库已更新到 2.0.7。本轮按现有原条目同步外部分发渠道，协议保持 2。合集源提交 `fffcacbb99e8ad1f79532595acbb7fc0299529ed`，专用仓库源提交 `230f61b2e7da71fb09f9126b9a1f81d4d95a79ef`；11 个 Skill 文件及 `.skill` 安装包已逐字节核对一致。7 个渠道适配 ZIP 递归本地预检全部 PASS。

| 渠道 | 核验结果 |
|---|---|
| [Agensi](https://www.agensi.io/skills/whobot-ai-phone-agents) | 原条目 2.0.7 更新已上线，平台 revision 1.1，公开更新日志和 12 文件结构可见，显示安全扫描通过；编辑页回读为 2.0.7 ZIP。 |
| [ClawHub 个人](https://clawhub.ai/whobot-ai/whobot-phonecall-skill) | latest 2.0.7；versionId `k974t931pxpeqr7e5pt814xtmn8hwp4r`，11 源文件哈希一致。版本级 security suspicious / hasWarnings，Skillspector MEDIUM 32、ClawScan suspicious。 |
| [ClawHub 组织](https://clawhub.ai/whobot/whobot-phonecall-skill) | latest 2.0.7；versionId `k975szjyc7783sns6rtv5hb1q18hwpfs`，11 源文件哈希一致。版本级 security clean / hasWarnings，Skillspector MEDIUM 23、ClawScan clean。 |
| [ModelScope](https://modelscope.cn/skills/WhoBot/whobot-phonecall-skill) | 手动同步成功，公开 ZIP 的 11 文件与源码一致；自动同步继续开启。 |
| [GitCode v2.0.7](https://gitcode.com/whobot/openclaw-whobot-skill/releases/v2.0.7) | 原仓库提交 `caaf8eaa2bab7da019ff439b6b74a0c000ad212d`，保留历史及独立 MCP。发行版已发布，重新下载附件与上游包一致。 |
| [skills.sh](https://skills.sh/whobot-ai/skills/whobot-phonecall-skill) | 单次隔离安装 2.0.7 成功，11 文件一致；公开页面仍显示 2.0.6，未声称索引已刷新。扫描：Gen safe、Socket 0 alerts、Snyk medium。 |
| [SkillHub.club](https://skillhub.club/skills/langgpt-whobot-phonecall-skill) | 原条目 revision 3，完整 12 文件回读一致，2.0.7 发布请求已受理，安全审核 queued；公开 pending_eval，尚未确认上线。 |
| WorkBuddy | 原 2.0.6 审核中，仅可撤回；未撤回。2.0.7 候选包已准备，未提交。 |
| SkillHub 企业版 | 2.0.7 上传解析 12 文件后提交，被“已有版本审核中”拒绝；原 2.0.6 仍安全审核中，公开 2.0.5。未跳过安全审核或创建替代条目。 |
| 千问企业版 | 原 2.0.6 因需要本次版本完整实测视频而运行测评失败；安全/内容检查通过。2.0.7 包已准备，未复用历史视频重提，未执行付费业务。 |
| 豆包 | 原 2.0.5 驳回：11.6 暂停三方纯 Skill，要求 MCP + Plugin。2.0.7 候选包准备完成，未扩展为插件改造。 |
| 百度 DuMate | 原平台 1.0.0 / 源 2.0.4 仍审核中、更新禁用；2.0.7 包已准备，未撤回。 |
| 其他目录 | SkillHaven 原仓库申请仍 In review；SkillsDirectory 原条目索引为 2.0.6；SkillsLLM 原仓库申请不重复提交；Skillry 2.0.7 单次提交 Failed to fetch、个人页仍 0 Skills；agentskill.sh 导航超时。 |

两份 ClawHub 官方报告 ZIP 和 manifest 已保存，均为 2.0.7 / 11 文件 / sourceKind published / status succeeded。报告声明部分扫描覆盖受限；主要提示可变 CLI 版本和业务资料/录音的隐私说明。平台 verdict 与本地预检分开记录。

安装包 / GitCode 附件 SHA-256：`99cbf8daa799c909cbe671eff2847078caf64aad6aa2b0b12f63d68d18eb0425`。

控制机证据：`whobot-mcp-app/auto/artifacts/releases/2026-11-09-whobot-2.0.7/`，入口 `README.md` / `publication-results.json`；MacAir `/Users/jiangshu/whobot-release-2.0.7-20261109/`。本轮无真实拨号、员工创建、客户名单上传或收费业务，百度测试用例未在平台沙箱运行。未创建后台监控任务；待审核渠道后续继续原条目、原版本。
