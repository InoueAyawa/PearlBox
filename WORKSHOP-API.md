# PearlBox plugin API

Client **1.0.0** · Local API **v1** · [简体中文](WORKSHOP-API.md) · [Русский](WORKSHOP-API.ru.md) · [OpenAPI 3.1 reference](workshop.openapi.json)

To show hashrate, power and temperature together in the Advanced panel, all you need is a JSON file. Version 1.0.0 accepts **read-only monitoring plugins**: their files describe which readings to display and cannot run scripts, commands or external programs. The fleet controller, managed device and XMR CPU miner are built into the client and use a different type of file for installation.

## Make a monitoring plugin

1. Download or copy [rig-monitor.pearlplugin](examples/rig-monitor.pearlplugin). It contains the UTF-8 JSON shown below. Change `name` to label your panel and put the readings you want in `metrics`, in display order.
2. In the client, open **Advanced → Plugins**, click **＋** on the right and select your file. The file picker accepts `.pearlplugin` and `.json` files of up to **12,000 bytes**.
3. Your panel now appears in the plugin list. To update it, edit and import the file again with the same `id`. A new `id` adds a separate panel. You can store up to 32 local monitoring plugins.

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

| Field | Accepted value |
| --- | --- |
| `pearlboxPlugin`, `type` | The number `1` and the string `"monitor"`. Only the six fields in the example are accepted. |
| `id` | Must match `^local-[a-z0-9][a-z0-9-]{0,40}$`, e.g. `local-rig-monitor`. |
| `name` | Up to 60 UTF-16 code units, no control characters U+0000–U+001F, and not blank after trimming. |
| `version` | Three numeric components of 1–4 digits each, e.g. `1.0.0`. |
| `metrics` | Choose 1–5 distinct readings: `hashrate` (total), `shares` (accepted/rejected), `power` (total GPU power), `temperature` (hottest GPU), `uptime` (current session duration). |

## Import an official plugin

| Descriptor | What it adds |
| --- | --- |
| [control-master.pearlplugin](examples/control-master.pearlplugin) | Fleet controller: sign in to the Control service, claim devices and manage jobs. |
| [control-agent.pearlplugin](examples/control-agent.pearlplugin) | Managed device: pair the device and confirm the account locally. Remote mining starts require separate permission on the device. |
| [cpu-xmr.pearlplugin](examples/cpu-xmr.pearlplugin) | Optional CPU mining of XMR during a GPU mining session. Prepare the engine, configure the pool and confirm the start. |

An official descriptor has only three fields: `pearlboxPlugin: 1`, `type: "builtin"` and `id` (the filename above without the extension). Importing installs the plugin. Select its name to open its settings. The installation persists across client restarts; enable the plugin again for the new session.

## Call the local API

Use paths relative to the client page; the port is not fixed. Send `Authorization: Bearer <session token>` using the value from the page’s `meta[name="api-token"]`. In Linux web mode, first open the full link printed by the launcher so the browser receives its HttpOnly session cookie. Keep tokens, private startup links and account credentials out of descriptor files. The local API, release website and Control service have separate authentication.

| Method and path | Request and result |
| --- | --- |
| `GET /api/v1/workshop` | Returns `apiVersion`, `plugins` (official catalog), `local` (custom monitors) and an optional `warning`. For each official plugin, `installedVersion: null` means it is not installed; `enabled` refers to this session and `state` holds the plugin’s current status. |
| `GET /api/v1/workshop/schema` | The OpenAPI reference for the running client. An [offline copy for 1.0.0](workshop.openapi.json) is included. |
| `POST /api/v1/workshop/import` | Send the descriptor itself as a JSON object, not as a form or file upload. Returns the full catalog. |
| `POST /api/v1/workshop/remove-local` | Example: `{"id":"local-rig-monitor"}`. Removes the monitor and returns the updated catalog. |
| `POST /api/v1/workshop/{id}/{action}` | Official plugins only. Send `{}` for `install` (also updates), `enable`, `disable` or `uninstall`. Install and enable the plugin before requesting any other action. |

Send POST requests with `Content-Type: application/json`. This read-only example lists plugins from the client page without enabling any:

```js
const response = await fetch('/api/v1/workshop', {
  headers: { Authorization: `Bearer ${document.querySelector('meta[name="api-token"]').content}` }
});
if (!response.ok) throw new Error(`HTTP ${response.status}`);
const { plugins, local } = await response.json();
console.table(plugins.map(({ id, installedVersion, enabled }) => ({ id, installedVersion, enabled })));
console.table(local.map(({ id, metrics }) => ({ id, metrics: metrics.join(', ') })));
```

| Official plugin | Actions (see OpenAPI for request fields) |
| --- | --- |
| `control-master` | `setup`, `login`, `logout`, `devices`, `claim`, `job`, `batch`, `history`, `revoke`, `accounts`, `create-account`, `vip`. Account management requires administrator privileges. |
| `control-agent` | `pair`, `check`, `confirm`, `allow-start`, `disconnect`. |
| `cpu-xmr` | `prepare`, `configure`, `start`, `stop`. `start` requires `{"confirm":true}` and a running GPU mining session. |

Errors return `{"error":"…"}`. For `400`, check the fields, plugin ID, action and enabled state. For `401`, refresh the client page to obtain its current token. For `403`, use the same client origin; in Linux web mode reopen the launcher link. For `404` or `405`, check the path and HTTP method.

General client routes such as `/api/state` (client status) and `/api/licenses` (notices) are outside the plugin API. A monitoring descriptor uses existing dashboard readings; it does not need to poll those routes.
