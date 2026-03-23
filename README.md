# edgetunnel-do

在 Cloudflare Workers 上运行的 VLESS-over-WebSocket 隧道，使用 Durable Object 固定出口 IP 地区，支持 ProxyIP 回退。

## 特性

- **Durable Object 区域固定**：通过 `locationHint` 控制出口 IP 所属地理大区
- **ProxyIP 回退**：直连失败时自动通过 ProxyIP 中转重试
- **UDP DNS 代理**：端口 53 的 DNS 查询通过 DoH（DNS-over-HTTPS）转发
- **订阅生成**：访问 `/{UUID}` 自动生成客户端订阅配置

## 一键部署

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/zzzxxxxxxxxxx/edgetunnel-do)

## 环境变量

| 变量 | 必需 | 默认值 | 说明 |
|------|------|--------|------|
| `UUID` | ✅ | — | VLESS 用户 UUID |
| `PROXYIP` | ❌ | `[2602:fc59:b0:64::6810:0]` | 回退代理 IP（默认通过 NAT64 网关访问 104.16.0.0） |
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

## 致谢

- [zizifn/edgetunnel](https://github.com/zizifn/edgetunnel) - 原始项目
- [juerson/3h1-dowss](https://github.com/juerson/3h1-dowss) - Durable Object 固定出口 IP 地区
