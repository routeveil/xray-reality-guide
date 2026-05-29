# Config Examples

Ready-to-use configuration files for Xray Reality. Replace the placeholder values with your own.

---

## Server config

`/usr/local/etc/xray/config.json`

```json
{
  "log": {
    // "warning" is sufficient for normal use
    // Change to "debug" temporarily if troubleshooting
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "listen": "0.0.0.0",   // Listen on all interfaces
      "port": 443,            // Use 443 — looks like normal HTTPS
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "YOUR-UUID-HERE",           // Generate with: xray uuid
            "flow": "xtls-rprx-vision"        // Required for Reality
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "show": false,
          // dest: the cover domain — traffic will mimic TLS to this site
          // Must be a real site that supports TLS 1.3
          // You do NOT need to own this domain
          "dest": "www.microsoft.com:443",
          "xver": 0,
          "serverNames": [
            "www.microsoft.com"   // Must match dest domain
          ],
          // privateKey: from "xray x25519" — keep this secret
          "privateKey": "YOUR-PRIVATE-KEY-HERE",
          "shortIds": [
            ""   // Leave empty or add a short hex string for extra security
          ]
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",   // Route traffic out normally
      "tag": "direct"
    }
  ]
}
```

> **Note:** Standard JSON does not support comments (`//`). Remove all comment lines before using this config. The comments above are for explanation only.

---

## Client config (v2rayNG / Hiddify / sing-box)

For desktop clients that accept raw JSON config:

```json
{
  "log": {
    "loglevel": "warning"
  },
  "inbounds": [
    {
      // Local SOCKS5 proxy — set this in your browser or system proxy settings
      "listen": "127.0.0.1",
      "port": 1080,
      "protocol": "socks",
      "settings": {
        "udp": true
      }
    },
    {
      // Local HTTP proxy — use this for apps that don't support SOCKS5
      "listen": "127.0.0.1",
      "port": 1081,
      "protocol": "http"
    }
  ],
  "outbounds": [
    {
      "protocol": "vless",
      "settings": {
        "vnext": [
          {
            "address": "YOUR-SERVER-IP",   // Your VPS IP address
            "port": 443,
            "users": [
              {
                "id": "YOUR-UUID-HERE",         // Must match server config
                "flow": "xtls-rprx-vision",
                "encryption": "none"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          // serverName: your cover domain (same as server's dest)
          "serverName": "www.microsoft.com",
          // fingerprint: browser TLS fingerprint to mimic
          // "chrome" is a safe default
          "fingerprint": "chrome",
          // publicKey: from "xray x25519" — the PUBLIC key (not private)
          "publicKey": "YOUR-PUBLIC-KEY-HERE",
          "shortId": "",
          "spiderX": ""
        }
      },
      "tag": "proxy"
    },
    {
      "protocol": "freedom",
      "tag": "direct"
    }
  ],
  "routing": {
    "rules": [
      {
        // Route local and private IPs directly, not through proxy
        "ip": ["geoip:private"],
        "outboundTag": "direct"
      }
    ]
  }
}
```

---

## Share link format

To import a connection into Shadowrocket, v2rayNG, or any VLESS-compatible client:

```
vless://YOUR-UUID@YOUR-SERVER-IP:443?encryption=none&flow=xtls-rprx-vision&security=reality&sni=www.microsoft.com&fp=chrome&pbk=YOUR-PUBLIC-KEY&type=tcp&headerType=none#MyServer
```

**Parameters:**
| Parameter | Value | Description |
|---|---|---|
| `encryption` | `none` | VLESS doesn't encrypt independently — TLS handles it |
| `flow` | `xtls-rprx-vision` | Required for Reality |
| `security` | `reality` | Protocol type |
| `sni` | cover domain | Must match server's `serverNames` |
| `fp` | `chrome` | TLS fingerprint — `chrome`, `firefox`, `safari` all work |
| `pbk` | public key | From `xray x25519` output |
| `type` | `tcp` | Transport |

Generate a QR code from this link to scan directly in Shadowrocket or v2rayNG.

---

## Multiple users

To add a second user (e.g. for a family member), add another entry to the `clients` array:

```json
"clients": [
  {
    "id": "FIRST-UUID",
    "flow": "xtls-rprx-vision"
  },
  {
    "id": "SECOND-UUID",    // Generate a new UUID: xray uuid
    "flow": "xtls-rprx-vision"
  }
]
```

Each user gets their own UUID but uses the same server IP and public key.

---

## Cover domain alternatives

If `www.microsoft.com` is blocked or behaves unexpectedly:

| Domain | Notes |
|---|---|
| `www.apple.com` | Reliable, global |
| `addons.mozilla.org` | Good TLS 1.3 support |
| `dl.google.com` | Works well from China |
| `www.cloudflare.com` | Fast, reliable |
| `ajax.googleapis.com` | Good fallback |

**How to test a cover domain:**
```bash
curl -I --tlsv1.3 https://www.microsoft.com
```
Should return `HTTP/2 200` or `301`. If it times out or returns an error, choose a different domain.

---

## Don't want to configure this yourself?

**RouteVeil** does the full setup — cover domain selection, key generation, server config, and client QR codes for all your devices.

One-time $99. Your server, your IP.

→ [routeveil.com](https://routeveil.com) · [t.me/routeveil](https://t.me/routeveil)
