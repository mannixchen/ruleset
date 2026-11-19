# Network Ruleset (`mannixchen/ruleset`)

多设备（Surge Mac / Surge iOS / Stash）共用的公开通用分流规则库。

## 设计约束

1. **纯匹配条件，不含策略列**：所有 `.list` 文件仅声明匹配类型与目标（如 `DOMAIN-SUFFIX,example.com` 或 `IP-CIDR,192.168.0.0/16,no-resolve`），具体走哪条策略组由客户端主配置决定。
2. **通用域名与桌面进程分离**：
   - `surge/*.list`：纯域名与 IP 规则，三端 Surge 与 Stash (`classical` / `text`) 均可直接订阅。
   - `mac/*.list`：仅 macOS 桌面端使用的 `PROCESS-NAME` 进程规则。
3. **不含任何私有/敏感信息**：家庭私有服务、公司内网、Tailscale 网段及代理订阅凭据均不放入本仓库。

## 文件清单与推荐绑定策略

### 1. 通用域名与网段 (`surge/`)

| 文件 | 用途 | 推荐策略组 | Raw 地址 |
|---|---|---|---|
| `surge/google_photos.list` | Google Photos 网页与专用上传 API（前置优先于通用 Google） | `🏪️ 云盘储存` | `https://raw.githubusercontent.com/mannixchen/ruleset/main/surge/google_photos.list` |
| `surge/cloud_storage.list` | Google Drive / PikPak / Box / pCloud 核心域名（前置大流量例外） | `🏪️ 云盘储存` | `https://raw.githubusercontent.com/mannixchen/ruleset/main/surge/cloud_storage.list` |
| `surge/proxy_yes.list` | 自定义强制走代理的网址域名 | `🚀 节点选择` | `https://raw.githubusercontent.com/mannixchen/ruleset/main/surge/proxy_yes.list` |
| `surge/direct_yes.list` | 自定义强制直连的网址、路由器管理域名与局域网保留网段 | `DIRECT` | `https://raw.githubusercontent.com/mannixchen/ruleset/main/surge/direct_yes.list` |
| `surge/reject_yes.list` | 自定义广告、遥测与追踪拦截黑名单 | `REJECT` | `https://raw.githubusercontent.com/mannixchen/ruleset/main/surge/reject_yes.list` |

### 2. macOS 桌面进程 (`mac/`)

| 文件 | 用途 | 推荐策略组 |
|---|---|---|
| `mac/cloud_apps.list` | Google Photos / Drive / OneDrive / Dropbox / MEGA / PikPak / Box / pCloud 客户端进程 | `🏪️ 云盘储存` |
| `mac/ai_apps.list` | Claude / Claude Code / OpenCode 进程 | `🤖 AI 服务` |
| `mac/google_apps.list` | Antigravity / Gemini 桌面端（遵循“Google 系只走谷歌服务”） | `🔍 谷歌服务` |
| `mac/proxy_apps.list` | Spotify / Telegram / Steam / Surge / Music 进程 | `🚀 节点选择` |
| `mac/direct_apps.list` | 钉钉 / 百度网盘 / 网速管家 / 剪映 / 腾讯会议 / 阿里云盘 / 115 / MindMaster 进程 | `DIRECT` |
| `mac/reject_apps.list` | 搜狗输入法等禁止联网进程 | `REJECT` |
