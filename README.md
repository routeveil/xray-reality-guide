# Xray Reality VPN Guide

![GitHub stars](https://img.shields.io/github/stars/routeveil/xray-reality-guide?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)
![Works in China](https://img.shields.io/badge/works%20in-China%20🇨🇳-red?style=flat-square)
![Works in Russia](https://img.shields.io/badge/works%20in-Russia%20🇷🇺-blue?style=flat-square)
![Protocol](https://img.shields.io/badge/protocol-Xray%20Reality-purple?style=flat-square)

How to set up a private VPN server that actually works in China, Russia, Iran and Turkey — using the Xray Reality protocol.

[English](README.md) · [中文](README.zh.md) · [Русский](README.ru.md)

---

## Why this exists

Commercial VPNs (ExpressVPN, NordVPN, etc.) keep getting blocked because they share IP addresses across thousands of users. The Great Firewall and Russia's TSPU detect the traffic patterns and block the IPs in bulk.

In February 2026, Russia added protocol-level blocking for WireGuard, OpenVPN, and standard IKEv2 nationwide.

Xray Reality works differently. Instead of tunneling traffic inside a detectable VPN protocol, it performs a genuine TLS 1.3 handshake with a real legitimate domain. The firewall inspects the connection, sees valid HTTPS traffic, and passes it through.

---

## Quick start

👉 **[Full setup guide → SETUP.md](SETUP.md)**

👉 **[Config file examples → CONFIG-EXAMPLES.md](CONFIG-EXAMPLES.md)**

---

## What you need

- A VPS from any major provider (~$5/month) — Hetzner, Vultr, Contabo
- A "cover" domain for the TLS handshake (you don't need to own it)
- Xray Core installed on the server
- A client app: Shadowrocket (iOS), v2rayNG (Android), Hiddify (Desktop)

---

## Why it works where WireGuard doesn't

WireGuard uses UDP with a distinctive handshake pattern — China and Russia detect and block it within seconds. Reality performs a real TLS session with a legitimate domain. Deep packet inspection confirms it looks like normal HTTPS traffic, because it technically is.

📖 Full technical explanation: [routeveil.com/blog/xray-reality-vs-wireguard-china.html](https://routeveil.com/blog/xray-reality-vs-wireguard-china.html)

---

## Recommended VPS locations

| Location | Provider | Why |
|---|---|---|
| Japan / Singapore | Vultr, Linode | Low latency from China |
| Frankfurt | Hetzner | Good for Russia/Turkey/Iran |
| Netherlands | Contabo | Affordable, reliable |

---

## Don't want to set it up yourself?

Configuration requires server-side setup — choosing the right cover domain, configuring certificates, matching client parameters. Any misconfiguration makes it detectable or non-functional.

**RouteVeil does the full setup for you** — one-time $99, your server, your IP, no recurring fees.

→ [routeveil.com](https://routeveil.com)  
→ Telegram: [t.me/routeveil](https://t.me/routeveil)

---

## Related reading

- [Why your VPN stops working in China every few months](https://routeveil.com/blog/why-vpn-stops-working-china.html)
- [Russia blocked Telegram. Then their banking system crashed.](https://routeveil.com/blog/russia-blocked-telegram-vpn-banking-crash.html)
- [Xray Reality vs WireGuard: why one works and the other doesn't](https://routeveil.com/blog/xray-reality-vs-wireguard-china.html)

---

## Resources

- [Xray Core on GitHub](https://github.com/XTLS/Xray-core)
- [XTLS documentation](https://xtls.github.io/)
