# edgetunnel-nat64-do

在 Cloudflare Workers 上运行的 VLESS-over-WebSocket 隧道，包含 NAT64 回退逻辑以解决与 Cloudflare CDN 的直连问题，使用 Durable Object 固定出口 IP 地区。

## 特性

- **Durable Object 区域固定**：通过 `locationHint` 控制出口 IP 所属地理大区
- **NAT64 回退**：直连失败时自动将目标 IPv4 转换为 NAT64 IPv6 地址重试
- **DoH 多服务器轮询**：支持 Cloudflare、Google 等多个 DoH 服务器轮询，避免单一服务器速率限制
- **DNS 缓存**：NAT64 域名解析结果缓存（TTL 60-600s），减少重复 DoH 查询
- **DoH 自动重试**：单个 DoH 服务器失败时自动切换到下一个，最多尝试全部服务器
- **UDP DNS 代理**：端口 53 的 DNS 查询通过 DoH（DNS-over-HTTPS）转发
- **订阅生成**：访问 `/{UUID}` 自动生成客户端订阅配置

## 一键部署

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/zzzxxxxxxxxxx/edgetunnel-nat64-do)

## 环境变量

| 变量 | 必需 | 默认值 | 说明 |
|------|------|--------|------|
| `UUID` | ✅ | — | VLESS 用户 UUID |
| `NAT64_PREFIX` | ❌ | `2602:fc59:b0:64::` | NAT64 地址前缀 |
| `REGION` | ❌ | `wnam` | Durable Object 放置区域，影响出口 IP 所属大区 |
| `BESTIP` | ❌ | `saas.sin.fan` | 订阅生成时使用的优选 IP 或域名 |

### REGION 可选值

| 值 | 区域 |
|----|------|
| `wnam` | 北美西部 |
| `enam` | 北美东部 |
| `sam` | 南美 |
| `weur` | 西欧 |
| `eeur` | 东欧 |
| `apac` | 亚太 |
| `oc` | 大洋洲 |
| `afr` | 非洲 |
| `me` | 中东 |

DO 可以放置的位置：https://where.durableobjects.live/

## DoH 服务器列表

轮询使用以下 DoH 服务器，区分 wire（dns-message 二进制）和 json（dns-json）两种格式：

| 服务商 | Wire 格式 (UDP DNS 代理) | JSON 格式 (NAT64 解析) |
|--------|--------------------------|------------------------|
| Cloudflare | `https://cloudflare-dns.com/dns-query` | `https://cloudflare-dns.com/dns-query` |
| Google | `https://dns.google/dns-query` | `https://dns.google/resolve` |

> **注意**：Google 的 JSON 格式端点是 `/resolve`，与其他服务商的 `/dns-query` 不同。

## 致谢

- [zizifn/edgetunnel](https://github.com/zizifn/edgetunnel) - 原始项目
- [cylind/awesome_cloudflare_workers/worker-vless-nat64.js](https://github.com/cylind/awesome_cloudflare_workers/blob/main/worker-vless-nat64.js) - NAT64转换
- [juerson/3h1-dowss](https://github.com/juerson/3h1-dowss) - Durable Object 固定出口 IP 地区
