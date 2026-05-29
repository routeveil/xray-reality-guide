# Xray Reality VPN 配置指南

如何搭建一台真正能在中国使用的私人VPN服务器——基于Xray Reality协议。

[English](README.md) · 中文 · [Русский](README.ru.md)

---

## 为什么要写这个指南

ExpressVPN、NordVPN等商业VPN不断被封锁，原因是它们把IP地址共享给成千上万的用户。防火长城识别出流量特征，批量封锁这些IP。

Xray Reality的工作方式完全不同。它不是把流量包在一个可识别的VPN协议里，而是用一个真实的合法域名完成TLS 1.3握手。防火长城检查连接，看到的是正常的HTTPS流量，于是放行。

---

## 你需要准备什么

- 一台境外VPS（约$5/月）——Hetzner、Vultr、Contabo都可以
- 一个用于TLS握手的"伪装"域名（你不需要拥有它）
- 服务器上安装Xray Core
- 客户端应用：Shadowrocket（iOS）、v2rayNG（Android）、Hiddify（桌面端）

---

## 为什么它能过GFW，而WireGuard不行

WireGuard使用UDP，有明显的握手特征——中国几秒内就能识别并封锁。Reality执行的是与合法域名的真实TLS会话。深度包检测确认流量看起来像正常HTTPS——因为从技术上来说它确实是。

📖 完整技术解析：[routeveil.com/blog/zh-xray-reality-vs-wireguard.html](https://routeveil.com/blog/zh-xray-reality-vs-wireguard.html)

---

## 推荐VPS服务商

| 地区 | 服务商 | 适用场景 |
|---|---|---|
| 日本 / 新加坡 | Vultr、Linode | 中国用户低延迟首选 |
| 法兰克福 | Hetzner | 俄罗斯/土耳其/伊朗 |
| 荷兰 | Contabo | 性价比高，稳定 |

---

## 快速开始

👉 [完整配置教程 (SETUP.md)](SETUP.md)

👉 [配置文件示例 (CONFIG-EXAMPLES.md)](CONFIG-EXAMPLES.md)

---

## 不想自己配置？

服务端配置需要选择合适的伪装域名、生成密钥对、匹配客户端参数。任何一个环节出错都会导致连接失败或被识别。

**RouteVeil 帮你全程配置**——一次性$99，在你自己的服务器上，你的IP，无月费。

→ [routeveil.com](https://routeveil.com)  
→ Telegram：[t.me/routeveil](https://t.me/routeveil)

---

## 延伸阅读

- [为什么你的VPN每隔几个月就失效](https://routeveil.com/blog/zh-weishenme-vpn-zongshi-shixiao.html)
- [Xray Reality vs WireGuard：为什么一个能翻墙，另一个不能](https://routeveil.com/blog/zh-xray-reality-vs-wireguard.html)
- [Shadowrocket共享账号的真实风险](https://routeveil.com/blog/zh-shadowrocket-gongxiang-ziji-fuwuqi.html)

---

## 相关资源

- [Xray Core GitHub](https://github.com/XTLS/Xray-core)
- [XTLS 官方文档](https://xtls.github.io/)
