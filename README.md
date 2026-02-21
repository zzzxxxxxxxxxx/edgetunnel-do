edgetunnel-nat64-do

在 Cloudflare Workers 上运行的 VLESS-over-WebSocket 隧道，包含 NAT64 回退逻辑以解决与 Cloudflare CDN 的直连问题，使用Durable Object固定出口IP地区

部署：

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/zzzxxxxxxxxxx/edgetunnel-nat64-do)

环境变量：
- `UUID`：必需，VLESS 用户 UUID。
- `NAT64_PREFIX`：可选，生成 NAT64 地址的前缀，默认 `2602:fc59:b0:64::`。
- `REGION`：用以在首次实例化时通过 `locationHint` 控制 Durable Object 放置区域，从而影响出口 IP 所属大区。可选值：`wnam`, `enam`, `sam`, `weur`, `eeur`, `apac`, `oc`, `afr`, `me`，默认 `wnam`。
- `BESTIP`：可选，订阅（subscription）生成时使用的优选 IP 或域名，默认 `saas.sin.fan`。设置后订阅中的服务器地址将使用此值而不是请求的主机名。

DO可以放置的位置： https://where.durableobjects.live/
