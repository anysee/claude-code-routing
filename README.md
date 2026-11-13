# AI 分流规则

面向 Clash.Meta / Mihomo 的 OpenAI 与 Anthropic/Claude AI 服务规则集，使用 `payload:` 格式。它是规则提供者文件，需要由主配置引用，不是可独立运行的完整 Clash 配置。

## 覆盖范围

- OpenAI 与 ChatGPT 相关域名
- Anthropic、Claude 与 Claude Code 相关域名
- GeoSite 兜底规则：`anthropic`、`category-ntp`
- 相关 CDN、认证、遥测、Persona 身份验证域名及 IP 兜底规则

规则会交给主配置中 `RULE-SET` 指定的策略组处理。请先在主配置中准备好对应的策略组。

## 使用方式

在主配置的 `rule-providers` 中添加规则提供者，并将 `RULE-SET` 规则放在通用规则之前：

```yaml
rule-providers:
  ai:
    type: http
    behavior: classical
    format: yaml
    path: ./rule/claude/claude-code-routing.yaml
    url: https://raw.githubusercontent.com/anysee/claude-code-routing/main/rule/claude/claude-code-routing.yaml
    interval: 86400

rules:
  - RULE-SET,ai,🤖 AI服务
  # 其他通用规则放在后面
```

规则集内部先匹配明确列出的域名（包括 KYC 域名），再执行 GeoSite 兜底，最后执行 IP 兜底。这样 GeoSite 不会抢在具体域名规则之前匹配；主配置中也应将此 `RULE-SET` 放在通用规则之前。

请将 `🤖 AI服务` 替换为你主配置中实际存在的策略组名称。使用 `GEOSITE` 规则时，确保 Mihomo 已配置可用的 GeoSite 数据；使用 `IP-ASN` 规则时，确保相应 ASN 数据库可用。`category-ntp` 会把匹配到的 NTP 域名也交给该策略组，网络策略需允许所需的 UDP/NTP 流量。

Persona 身份验证（KYC）域名规则已启用。如不需要分流相关验证流量，可从规则文件中移除对应的 `maps.googleapis.com`、`withpersona.com` 和 `frompersona.com` 规则。共享认证域名部分的 `DOMAIN-SUFFIX,auth0.com` 已覆盖 `anthropic.auth0.com`，因此没有重复列出该精确域名。

## 说明

本项目仅供学习与研究 Clash/Mihomo 规则格式和域名分流使用，不是 OpenAI、Anthropic、Claude 或 Net.Coffee 的官方配置，也不保证服务可用性、连接质量或账户状态。使用前请自行核对规则、代理能力及相关服务条款。规则标识使用 `ai`；仓库路径保持稳定。

## 致谢

规则整理参考了以下 Net.Coffee 资料：

- [Claude Code 域名分流规则大全 - 域名列表配置](https://ip.net.coffee/claude/site.html)
- [Claude Code 稳定使用指南 - 减少风控 - 配置优化](https://ip.net.coffee/claude/claudecode.html)

感谢资料作者整理域名与配置背景。OpenAI、Anthropic、Claude 及相关商标归其各自权利人所有；本项目与上述公司和 Net.Coffee 均无关联。

## License

本项目采用 [MIT License](LICENSE)。
