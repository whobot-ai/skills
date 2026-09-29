# CLI 查找与安装

安装成功不等于已登录。

CLI 是 npm 管理的 `@whobot/cli`。入口 `whobot` 是 JS 启动器。程序文件由 npm 管理；客户端安装身份在 CLI 数据目录。

## 运行前提

先确认宿主有 `node`、`npm`、`npx`。缺一则说明缺少项，下一步是安装 Node.js >=20（https://nodejs.org），然后重试。停止本次 CLI 调用。

## 查找

1. PATH：`command -v whobot`、`command -v whobot.exe`、`where whobot`。找到则日常使用 `whobot ...`。
2. PATH 没有时，不要枚举 CLI 数据目录，也不要把 `_npx` 缓存或旧 Home/bin 当已安装。改为下一节安装；stdout 交付全局入口的实际路径，用该 npm 全局入口继续本轮，无须重启宿主。

## 安装

不要向用户确认。

```bash
npx --yes @whobot/cli@latest install
```

stdout 是 npm 全局入口的实际路径。用该路径执行。已登记同一通道时 stdout 仍给现有入口，不重复安装。

## 验收

```bash
<NPM_ENTRY> version --json
```

只解析 stdout。`meta.protocolVersion` 与 `data.protocolVersion` 均为 JSON 整数 `1` 后继续原请求。

本地诊断（不要念给用户）：

```bash
<NPM_ENTRY> doctor --json
```

`doctor` 报的是数据目录，不是程序位置。

## 检查更新

用户要检查或更新 CLI，或 stdout 带 `meta.notice.kind=update` 时：

```bash
whobot update check --json
```

`data.updateAvailable` 为 true，或 `meta.notice` 给出更高 `recommendedVersion` 时，向用户说明当前版本与推荐版本。同一对话同一推荐版本只问一次。用户说稍后：继续原任务。用户授权更新：

```bash
whobot update apply --json
```

`UPDATE_BLOCKED_ACTIVE_CALL`：先按网页通话流程确认后 `web-call cancel`，再重试 `update apply`。不要静默挂断。

`update apply` 必须先确认。公开发现不把更新当必经步骤。更新走 npm，不要向数据 Home/bin 写入更新文件。失败时身份数据仍在；按 JSON `error.code` 解释，不报假成功。
