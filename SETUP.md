# Xray Reality Setup Guide

Complete walkthrough — from a blank VPS to a working connection on your phone or laptop.

**Time required:** 20–30 minutes  
**Technical level:** Basic. If you can copy-paste commands into a terminal, you can do this.

---

## Step 1: Get a VPS

You need a small cloud server outside your country. ~$5/month.

**Recommended providers:**

| Provider | Location | Best for |
|---|---|---|
| [Hetzner](https://www.hetzner.com/cloud/) | Frankfurt, Helsinki | Russia, Turkey, Iran |
| [Vultr](https://www.vultr.com/) | Tokyo, Singapore, Seoul | China |
| [Contabo](https://contabo.com/) | Frankfurt, Singapore | Budget option |

**What to order:**
- Ubuntu 22.04 LTS (64-bit)
- 1 vCPU, 1GB RAM — sufficient
- Any storage size

After ordering, you'll receive an IP address, username (`root`) and password by email.

---

## Step 2: Connect to your server

**On Windows:** Download [PuTTY](https://www.putty.org/) or use Windows Terminal.

**On Mac/Linux:** Open Terminal.

```bash
ssh root@YOUR_SERVER_IP
```

Enter the password from your email. You're now inside your server.

---

## Step 3: Install Xray

Run the official installer:

```bash
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```

Verify it installed correctly:

```bash
xray version
```

You should see a version number like `Xray 1.8.x`.

---

## Step 4: Generate keys

Reality uses a key pair for authentication. Generate them:

```bash
xray x25519
```

You'll get output like this:

```
Private key: ABC123...
Public key:  XYZ789...
```

**Save both keys.** You'll need them in the config.

Also generate a UUID (your user ID):

```bash
xray uuid
```

Save this too.

---

## Step 5: Choose a cover domain

Reality performs a TLS handshake with a real domain. This domain is your "cover" — the firewall sees traffic going to it and passes it through.

**Requirements for the cover domain:**
- Must be a real, high-traffic website
- Must support TLS 1.3
- Must NOT be blocked in your country
- You do NOT need to own it — you're borrowing its TLS fingerprint

**Good choices:**
- `www.microsoft.com`
- `www.apple.com`
- `addons.mozilla.org`
- `dl.google.com`

Avoid: anything that might be blocked in your target country. Test at [https://www.toolsite.net/](toolsite.net) if unsure.

---

## Step 6: Configure the server

Create the config file:

```bash
nano /usr/local/etc/xray/config.json
```

Paste this configuration, replacing the placeholder values:

```json
{
  "log": {
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "listen": "0.0.0.0",
      "port": 443,
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "YOUR_UUID_HERE",
            "flow": "xtls-rprx-vision"
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "show": false,
          "dest": "www.microsoft.com:443",
          "xver": 0,
          "serverNames": [
            "www.microsoft.com"
          ],
          "privateKey": "YOUR_PRIVATE_KEY_HERE",
          "shortIds": [
            ""
          ]
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "tag": "direct"
    }
  ]
}
```

**Replace:**
- `YOUR_UUID_HERE` → the UUID from Step 4
- `YOUR_PRIVATE_KEY_HERE` → the private key from Step 4
- `www.microsoft.com` → your chosen cover domain (both in `dest` and `serverNames`)

Save: `Ctrl+X`, then `Y`, then `Enter`.

---

## Step 7: Start Xray

```bash
systemctl enable xray
systemctl start xray
systemctl status xray
```

You should see `active (running)` in green. If you see errors, check Step 6 — a typo in the JSON is the most common cause.

---

## Step 8: Open the firewall port

Allow traffic on port 443:

```bash
ufw allow 443
ufw enable
```

---

## Step 9: Configure your client

You need your connection details:

- Server IP
- Port: `443`
- UUID (from Step 4)
- Public key (from Step 4)
- Cover domain (from Step 5)
- Security: `reality`
- Flow: `xtls-rprx-vision`
- Network: `tcp`
- SNI: your cover domain

### iOS — Shadowrocket

1. Add new server → Type: `VLESS`
2. Fill in: host (your server IP), port `443`, UUID
3. Encryption: `none`, Flow: `xtls-rprx-vision`
4. Transport: `tcp`, TLS: `reality`
5. SNI: your cover domain, Public Key: your public key

### Android — v2rayNG

1. `+` → Manual input → VLESS
2. Fill in address, port, UUID
3. Flow: `xtls-rprx-vision`
4. Security: `reality`
5. SNI + Public Key as above

### Desktop — Hiddify

1. Add new profile → Manual
2. Use the same parameters
3. Or generate a share link on the server (see below)

---

## Step 10: Generate a QR code / share link

To easily add the connection to additional devices:

```bash
cat << EOF
vless://YOUR_UUID@YOUR_SERVER_IP:443?encryption=none&flow=xtls-rprx-vision&security=reality&sni=www.microsoft.com&fp=chrome&pbk=YOUR_PUBLIC_KEY&type=tcp#RouteVeil
EOF
```

Replace the placeholders. Paste this into any VLESS-compatible client or generate a QR code from it at [qr-code-generator.com](https://www.qr-code-generator.com/).

---

## Verification

Test your connection:

1. Connect in your client app
2. Visit [https://ipinfo.io](https://ipinfo.io) — you should see your server's IP, not your local IP
3. Try accessing a blocked site

---

## Troubleshooting

**Connection refused**
- Check that Xray is running: `systemctl status xray`
- Check the firewall: `ufw status`
- Verify port 443 is open on your VPS provider's control panel (some providers have a separate firewall)

**Connected but no traffic**
- Double-check the UUID matches exactly in server config and client
- Verify the public key in the client matches the output from Step 4

**Xray fails to start**
- Check config syntax: `xray run -config /usr/local/etc/xray/config.json`
- Common issue: trailing comma in JSON, or mismatched quotes

**Works locally but not from China/Russia**
- Your cover domain may be blocked. Try a different one from Step 5.
- Make sure you're using port 443, not a custom port

---

## Updating Xray

```bash
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
systemctl restart xray
```

---

## Don't want to do this yourself?

Configuration requires getting several details exactly right — cover domain selection, key matching, client parameters. Any misconfiguration makes the connection detectable or non-functional.

**RouteVeil does the full setup for you.** One-time $99, on your server, your IP.

→ [routeveil.com](https://routeveil.com)  
→ Telegram: [t.me/routeveil](https://t.me/routeveil)
