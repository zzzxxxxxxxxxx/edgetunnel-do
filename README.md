edgetunnel

在 Cloudflare Workers 上运行的 VLESS-over-WebSocket 隧道，包含 NAT64 回退逻辑以解决与 Cloudflare CDN 的直连问题。

部署：

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/zzzxxxxxxxxxx/edgetunnel)

环境变量：
- `UUID`：必需，VLESS 用户 UUID。
- `NAT64_PREFIX`：可选，生成 NAT64 地址的前缀，默认 `2602:fc59:b0:64::`。

分支说明：
- `main`：保持原始轻量 worker 实现（不使用 Durable Objects）。
- `durable-object`：此分支将完整逻辑放入单文件 `src/worker.js` 中并注册为 Durable Object（类名 `WsDo`），新增 `REGION` 环境变量用以在首次实例化时通过 `locationHint` 控制 Durable Object 放置区域，从而影响出口 IP 所属大区。

新增（在 `durable-object` 分支）环境变量：
- `REGION`：可选，指定 DO 首次创建时的区域提示（例如 `wnam`, `enam`, `sam`, `weur`, `eeur`, `apac`, `oc`, `afr`, `me`），默认 `wnam`。

部署（`durable-object` 分支）注意：
- `wrangler.toml` 中包含 `durable_objects` 绑定（`WsDo` -> `WS_DO`）和 `migrations` 声明。首次使用时会根据 `REGION` 与 `name`（包含 `REGION` 与 `UUID`）生成不同的 Durable Object 实例，`locationHint` 仅在第一次创建实例时生效；要改变实例位置需使用不同的 name 或删除旧实例并重新部署。

检查 DO 放置位置： https://where.durableobjects.live/
