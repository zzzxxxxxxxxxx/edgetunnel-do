edgetunnel

在 Cloudflare Workers 上运行的 VLESS-over-WebSocket 隧道，包含 NAT64 回退逻辑以解决与 Cloudflare CDN 的直连问题。

部署：

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/zzzxxxxxxxxxx/edgetunnel)

环境变量：
- `UUID`：必需，VLESS 用户 UUID。
- `NAT64_PREFIX`：可选，生成 NAT64 地址的前缀，默认 `2602:fc59:b0:64::`。
