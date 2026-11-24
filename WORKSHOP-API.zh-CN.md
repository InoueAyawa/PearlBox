# PearlBox 插件 API

客户端 **1.0.0** · 本地 API **v1** · [English](WORKSHOP-API.en.md) · [Русский](WORKSHOP-API.ru.md) · [OpenAPI 3.1 接口清单](workshop.openapi.json)

如果想在高级面板中同时看算力、功耗和温度，写一份 JSON 文件就够了。1.0.0 支持自制**只读监测插件**：文件描述要显示的指标，不执行脚本、命令或外部程序。远程主控、受控设备和 CPU 双挖是客户端已有的官方功能，通过另一类描述文件安装。

## 做一个监测插件

1. 下载或复制 [rig-monitor.pearlplugin](examples/rig-monitor.pearlplugin)。它是下面这段 UTF-8 JSON。改 `name` 可给面板起名；在 `metrics` 中按想要的显示顺序保留指标。
2. 在客户端打开 **高级 → 插件**，点击右侧的 **＋**，选择文件。支持 `.pearlplugin` 和 `.json`；界面接受的文件最大为 **12,000 字节**。
3. 导入后即可在插件栏看到名称与指标。改好后再次导入：相同 `id` 更新原条目，换一个 `id` 则新增条目。最多可保留 32 个本地监测插件。

```json
{
  "pearlboxPlugin": 1,
  "type": "monitor",
  "id": "local-rig-monitor",
  "name": "Rig monitor",
  "version": "1.0.0",
  "metrics": ["hashrate", "shares", "power", "temperature", "uptime"]
}
```

| 字段 | 规则 |
| --- | --- |
| `pearlboxPlugin`、`type` | 固定为数字 `1` 和字符串 `"monitor"`。只接受示例中的六个字段。 |
| `id` | 格式 `^local-[a-z0-9][a-z0-9-]{0,40}$`，例如 `local-rig-monitor`。 |
| `name` | 不超过 60 个 UTF-16 代码单元，不含控制字符 U+0000–U+001F；去掉首尾空白后不得为空。 |
| `version` | 三段数字，每段 1–4 位，例如 `1.0.0`。 |
| `metrics` | 1–5 项，不可重复。`hashrate` 总算力、`shares` 接受／拒绝份额、`power` 显卡总功耗、`temperature` 最高显卡温度、`uptime` 本次会话时长。 |

## 导入官方插件

| 描述文件 | 导入后能做什么 |
| --- | --- |
| [control-master.pearlplugin](examples/control-master.pearlplugin) | 主控端：登录 Control 服务，认领设备并管理任务。 |
| [control-agent.pearlplugin](examples/control-agent.pearlplugin) | 受控端：配对并在设备上确认账号。是否允许远程启动挖矿需在设备上另行设置。 |
| [cpu-xmr.pearlplugin](examples/cpu-xmr.pearlplugin) | 在 GPU 挖矿会话中选用 CPU 双挖 XMR；须先准备内核、配置矿池并确认启动。 |

官方描述文件只包含 `pearlboxPlugin: 1`、`type: "builtin"` 和表中对应的 `id`（不含扩展名）。导入只做安装；点击插件名称可打开设置。客户端重启后保留安装记录，官方插件需重新启用。

## 在本地调用 API

请求使用客户端页面的同源相对地址，无固定端口。请求头传 `Authorization: Bearer <当前会话令牌>`，由客户端页面的 `meta[name="api-token"]` 提供。Linux 网页模式先打开启动终端给出的完整链接，浏览器会取得本次会话的 HttpOnly cookie。无需把令牌、私有启动链接或账号信息写进插件文件。本地 API 与公网发布站、Control 服务的登录相互独立。

| 方法与路径 | 请求与结果 |
| --- | --- |
| `GET /api/v1/workshop` | 返回 `apiVersion`、`plugins`（官方目录）、`local`（自制监测插件）及可选的 `warning`。官方条目的 `installedVersion` 为 `null` 表示未安装；`enabled` 表示本次会话是否启用，`state` 是该插件当前状态。 |
| `GET /api/v1/workshop/schema` | 当前客户端实际支持的 OpenAPI 接口清单；本包附有 [1.0.0 离线副本](workshop.openapi.json)。 |
| `POST /api/v1/workshop/import` | 直接发送描述文件的 JSON 对象，无需表单或文件上传。成功后返回完整清单。 |
| `POST /api/v1/workshop/remove-local` | 示例：`{"id":"local-rig-monitor"}`。移除该监测插件，返回更新后的清单。 |
| `POST /api/v1/workshop/{id}/{action}` | 仅适用于官方插件。`install`（安装或更新）、`enable`、`disable`、`uninstall` 均传 `{}`；业务动作须先安装并启用。 |

POST 使用 `Content-Type: application/json`。以下示例在客户端页面中读取清单，不启用插件：

```js
const response = await fetch('/api/v1/workshop', {
  headers: { Authorization: `Bearer ${document.querySelector('meta[name="api-token"]').content}` }
});
if (!response.ok) throw new Error(`HTTP ${response.status}`);
const { plugins, local } = await response.json();
console.table(plugins.map(({ id, installedVersion, enabled }) => ({ id, installedVersion, enabled })));
console.table(local.map(({ id, metrics }) => ({ id, metrics: metrics.join(', ') })));
```

| 官方插件 | 可用业务动作（请求字段见 OpenAPI） |
| --- | --- |
| `control-master` | `setup`, `login`, `logout`, `devices`, `claim`, `job`, `batch`, `history`, `revoke`, `accounts`, `create-account`, `vip`。账号管理需要管理员权限。 |
| `control-agent` | `pair`, `check`, `confirm`, `allow-start`, `disconnect`。 |
| `cpu-xmr` | `prepare`, `configure`, `start`, `stop`；`start` 需要 `{"confirm":true}` 和运行中的 GPU 挖矿会话。 |

错误响应为 `{"error":"…"}`。`400`：检查描述文件字段、插件 ID、动作及启用状态；`401`：刷新客户端页面以取得当前令牌；`403`：确认请求来自同一客户端，Linux 网页模式可重新打开终端启动链接；`404` 或 `405`：核对路径和 HTTP 方法。

`/api/state`（客户端状态）、`/api/licenses`（许可）等通用客户端接口不属于插件 API。自制监测插件只需描述文件，读取界面现有指标不需要自行轮询这些接口。
