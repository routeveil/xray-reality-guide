# Xray Reality VPN Guide

How to set up a private VPN server that actually works in China, Russia, Iran and Turkey — using the Xray Reality protocol.

## Why this exists

Commercial VPNs (ExpressVPN, NordVPN, etc.) keep getting blocked because they share IP addresses across thousands of users. The Great Firewall and Russia's TSPU detect the traffic patterns and block the IPs in bulk.

Xray Reality works differently. Instead of tunneling traffic inside a detectable VPN protocol, it performs a genuine TLS 1.3 handshake with a real legitimate domain. The firewall inspects the connection, sees valid HTTPS traffic, and passes it through.

## What you need

- A VPS from any major provider (~$5/month) — [Hetzner](https://hetzner.com), [Vultr](https://vultr.com), [Contabo](https://contabo.com)
- A domain or a "cover" domain for the TLS handshake
- [Xray Core](https://github.com/XTLS/Xray-core) installed on the server
- A client app: [Shadowrocket](https://apps.apple.com/us/app/shadowrocket/id932747118) (iOS), [v2rayNG](https://github.com/2dust/v2rayNG) (Android), [Hiddify](https://hiddify.com) (Desktop)

## Why it works where WireGuard doesn't

WireGuard uses UDP with a distinctive handshake pattern — China and Russia detect and block it within seconds. Reality performs a real TLS session with a legitimate domain. Deep packet inspection confirms it looks like normal HTTPS traffic, because it technically is.

Read the full technical explanation: [routeveil.com/blog/xray-reality-vs-wireguard-china.html](https://routeveil.com/blog/xray-reality-vs-wireguard-china.html)

## Recommended VPS locations

| Location | Provider | Why |
|----------|----------|-----|
| Japan / Singapore | Vultr, Linode | Low latency from China |
| Frankfurt | Hetzner | Good for Russia/Turkey/Iran |
| Netherlands | Contabo | Affordable, reliable |

## Don't want to set it up yourself?

Configuration requires server-side setup — choosing the right cover domain, configuring certificates, matching client parameters. Any misconfiguration makes it detectable.

**[RouteVeil](https://routeveil.com)** does the full setup for you — one-time $99, your server, your IP, no recurring fees.

→ [routeveil.com](https://routeveil.com)  
→ Telegram: [t.me/routeveil](https://t.me/routeveil)

## Related reading

- [Why your VPN stops working in China every few months](https://routeveil.com/blog/why-vpn-stops-working-china.html)
- [Russia blocked Telegram. Then their banking system crashed.](https://routeveil.com/blog/russia-blocked-telegram-vpn-banking-crash.html)
- [Xray Reality vs WireGuard: why one works and the other doesn't](https://routeveil.com/blog/xray-reality-vs-wireguard-china.html)

## Resources

- [Xray Core on GitHub](https://github.com/XTLS/Xray-core)
- [XTLS documentation](https://xtls.github.io)
