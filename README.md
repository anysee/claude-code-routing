# Claude Code 分流规则

面向 Clash.Meta / Mihomo 的 Claude Code 域名与 IP 规则集，采用 `payload:` 格式。它是规则集文件，不是可独立运行的完整 Clash 配置。

## 使用方式

在主配置中添加规则提供者，并把 `RULE-SET` 规则放在通用规则之前。将策略组名称替换成你配置中实际存在的名称：

```yaml
rule-providers:
  claude-code:
    type: http
    behavior: classical
    format: yaml
    path: ./rule/claude/claude-code-routing.yaml
    url: https://raw.githubusercontent.com/anysee/claude-code-routing/main/rule/claude/claude-code-routing.yaml
    interval: 86400

rules:
  - RULE-SET,claude-code,🤖 AI服务
  # 其他通用规则放在后面
```

确保主配置使用 `mode: rule`。GeoSite 规则需要 Mihomo 有可用的 GeoSite 数据库；`IP-ASN` 规则需要可用的 ASN 数据库。关键词规则 `datadog`、`sentry`、`sift` 是较宽泛的兜底，可能匹配到无关域名。

KYC 规则已启用，会匹配 Google Maps 和 Persona 域名并送往所选策略组；若不需要分流验证相关流量，可按需移除这三条规则。`GEOSITE,category-ntp` 会将匹配到的 NTP 域名流量送往所选策略组，可能影响设备时间同步；请确认该策略组的网络出口允许 UDP/NTP 流量。

## 说明

本项目用于学习和研究 Clash/Mihomo 规则集格式与域名分流。仅供学习参考，不是 Anthropic 或 Net.Coffee 的官方配置，也不保证服务可用性、连接质量或账户状态。使用前请自行核对规则、代理能力及相关服务条款。

## 致谢

规则整理参考了以下 Net.Coffee 资料：

- [Claude Code 域名分流规则大全 - 域名列表配置](https://ip.net.coffee/claude/site.html)
- [Claude Code 稳定使用指南 - 减少风控 - 配置优化](https://ip.net.coffee/claude/claudecode.html)

感谢资料作者整理域名与配置背景。Anthropic、Claude 及相关商标归其各自权利人所有；本仓库与 Anthropic、Net.Coffee 均无关联。

## License

本仓库原创配置与说明采用 [MIT License](LICENSE)。来源页面及其原文不因此转让或改变其权利归属。
