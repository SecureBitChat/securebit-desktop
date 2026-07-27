<div align="center">

<img src="logo/securebit-mark.svg" alt="SecureBit.chat" width="110">

# SecureBit Desktop

**Native desktop applications for SecureBit Chat — a zero-server, peer-to-peer messenger with end-to-end encryption.**

[Downloads](#downloads) · [Installing](#installing) · [Automatic updates](#automatic-updates) · [Verifying your download](#verifying-your-download) · [Security](#security-and-trust) · [Privacy](#privacy)

</div>

---

## Downloads

Current release: **0.3.0**

| Platform | Requirements | File |
|---|---|---|
| Windows | Windows 10 (1809+) or 11, x64 | [SecureBit.Chat_0.3.0_x64-setup.exe](https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.3.0/SecureBit.Chat_0.3.0_x64-setup.exe) |
| macOS | macOS 11+, Intel or Apple Silicon | [SecureBit.Chat_0.3.0_x64.dmg](https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.3.0/SecureBit.Chat_0.3.0_x64.dmg) |
| Linux | glibc 2.31+ (Ubuntu 20.04+, Debian 11+, Fedora 35+) | [SecureBit.Chat_0.3.0_amd64.AppImage](https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.3.0/SecureBit.Chat_0.3.0_amd64.AppImage) |

The macOS build is compiled for Intel and runs on Apple Silicon through Rosetta.

All releases are on the [releases page](https://github.com/SecureBitChat/securebit-desktop/releases).

---

## Installing

### Windows

1. Run `SecureBit.Chat_0.3.0_x64-setup.exe`.
2. SmartScreen may warn about an unrecognised app — choose **More info** → **Run anyway**.
3. Follow the installer. It lands in `C:\Program Files\SecureBit Chat\`.
4. Allow network access if Windows Firewall asks; the app needs it to reach peers directly.

### macOS

1. Open the `.dmg` and drag **SecureBit Chat** into Applications.
2. On first launch, right-click the app and choose **Open** — a plain double-click will be blocked by Gatekeeper.
3. Grant microphone and camera access when prompted, if you intend to use calls.

If the app still refuses to start:

```bash
xattr -d com.apple.quarantine "/Applications/SecureBit Chat.app"
```

### Linux

```bash
wget https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.3.0/SecureBit.Chat_0.3.0_amd64.AppImage
chmod +x SecureBit.Chat_0.3.0_amd64.AppImage
./SecureBit.Chat_0.3.0_amd64.AppImage
```

To add it to your application menu:

```bash
sudo mv SecureBit.Chat_0.3.0_amd64.AppImage /opt/securebit-chat.AppImage

cat > ~/.local/share/applications/securebit-chat.desktop <<'EOF'
[Desktop Entry]
Name=SecureBit Chat
Exec=/opt/securebit-chat.AppImage
Type=Application
Categories=Network;InstantMessaging;
EOF
```

---

## Automatic updates

The app checks for a new version shortly after launch, and on demand when you click the version number next to the SecureBit wordmark in the sidebar. When one is available you get a prompt: **Update now** downloads and installs it, then offers to restart.

**Why the update is trustworthy.** Every update bundle is signed with a key that never leaves the maintainer's machine, and the app verifies that signature against a public key compiled into the binary *before* installing anything. A bundle that is unsigned, altered, or signed by any other key is refused. Neither GitHub nor the network is part of the trust model — compromising either does not let anyone push code into your installation.

**Availability**

| Platform | Status |
|---|---|
| macOS | Automatic updates from 0.3.0 onward |
| Windows | Manual for now; automatic from the next build |
| Linux | Manual for now; automatic from the next build |

Version 0.1.0 predates the updater on every platform, so upgrading from it is a one-time manual download.

---

## Verifying your download

Compare the checksum of the file you downloaded against the list below.

```bash
# macOS / Linux
shasum -a 256 <file>
```

```powershell
# Windows
Get-FileHash .\SecureBit.Chat_0.3.0_x64-setup.exe -Algorithm SHA256
```

| File | SHA-256 |
|---|---|
| `SecureBit.Chat_0.3.0_x64-setup.exe` | `1483902008b5b1e1192b01af73d045f4bc3c2c5cb645c273c1cb63f048f74f62` |
| `SecureBit.Chat_0.3.0_x64.dmg` | `81e4e516692635aa13e927cd37420d3c1bd8a8d6aaec065b3291b8a63147cf59` |
| `SecureBit.Chat_0.3.0_amd64.AppImage` | `f3d6cc19d16b97a61b2cce16695084129f22f6842af8433a6fc7b007fe8316d0` |

The macOS build is additionally code-signed; you can inspect it with:

```bash
codesign -dvv "/Applications/SecureBit Chat.app"
```

---

## Security and trust

### The split

Everything that touches your messages, keys or connections is open source. The desktop applications are a wrapper around it.

```
┌──────────────────────────────────────┐
│  Desktop application (proprietary)   │
│  Native UI, platform integration,    │
│  window and notification handling,   │
│  update delivery                     │
└───────────────┬──────────────────────┘
                │ calls into
┌───────────────▼──────────────────────┐
│  securebit-core (open source)        │
│  Key exchange, encryption, protocol, │
│  verification, file transfer crypto  │
└──────────────────────────────────────┘
```

### What you can audit

| Repository | Contents | License |
|---|---|---|
| [securebit-core](https://github.com/SecureBitChat/securebit-core) | Every cryptographic operation and the full protocol implementation | Apache-2.0 |
| [securebit-chat](https://github.com/SecureBitChat/securebit-chat) | The complete web client, including its UI | MIT |

Start with [SECURITY_MODEL.md](https://github.com/SecureBitChat/securebit-core/blob/main/SECURITY_MODEL.md) and [THREAT_MODEL.md](https://github.com/SecureBitChat/securebit-core/blob/main/THREAT_MODEL.md) in the core repository — they state what is guaranteed and, just as importantly, what is not.

```bash
git clone https://github.com/SecureBitChat/securebit-core.git
cd securebit-core
cargo test
```

The test suite pins the protocol against the web implementation, so both clients cannot silently drift apart.

### How a session is secured

- Ephemeral ECDH P-384 key exchange, signed with ECDSA P-384, giving forward secrecy
- AES-256-GCM for messages and file chunks, with HMAC-SHA-256 over the payload
- A short verification code that both peers derive independently — comparing it out of band is what rules out an attacker in the middle
- No server sees your traffic: connections are peer-to-peer, and a relay is used only when a direct connection is impossible, where it forwards ciphertext it cannot read

### Reporting a vulnerability

Email security@securebit.chat rather than opening a public issue. Include a description, the affected version, and a reproduction if you have one. Responsible disclosure, 90-day window.

---

## Privacy

The applications collect nothing. There is no analytics, no telemetry, no usage statistics, and no crash reporting.

There is also nothing to collect on a server, because there is no server holding your data: no accounts, no stored messages, no contact lists, no connection logs, no keys.

---

## System requirements

| Platform | Minimum | Recommended |
|---|---|---|
| Windows | Windows 10 (1809+), 4 GB RAM, 200 MB disk | Windows 11, 8 GB RAM |
| macOS | macOS 11, 4 GB RAM, 200 MB disk | macOS 13+, 8 GB RAM |
| Linux | glibc 2.31+, 4 GB RAM, 200 MB disk | Ubuntu 22.04+ / Fedora 38+, 8 GB RAM |

Calls need a webcam and microphone. Bandwidth: roughly 128 kbps for audio, 1 Mbps for video.

Networking: the app uses WebRTC and needs outbound UDP for STUN (port 3478) and peer traffic. Restrictive firewalls fall back to a TLS relay on port 443.

---

## Status and roadmap

The desktop applications are in public beta. The cryptographic core is production-ready and shared with the web client; what is still being refined is platform integration and UI polish.

Planned:

- Automatic updates on Windows and Linux
- Distribution through the Microsoft Store, Mac App Store and Snap Store
- Mobile applications
- Group chats
- Post-quantum key exchange

Feature requests are tracked in [Issues](https://github.com/SecureBitChat/securebit-desktop/issues).

---

## Support and contributing

Questions and bug reports: [Issues](https://github.com/SecureBitChat/securebit-desktop/issues) or support@securebit.chat.

The desktop wrapper is not open source, but the parts that matter are, and contributions there are welcome — [securebit-core](https://github.com/SecureBitChat/securebit-core) for cryptography and protocol, [securebit-chat](https://github.com/SecureBitChat/securebit-chat) for the client itself.

---

## License

The desktop applications are proprietary and free for personal and commercial use. Redistribution requires permission; reverse engineering is not permitted. See [LICENSE](LICENSE).

The open-source components keep their own licenses: securebit-core under Apache-2.0, securebit-chat under MIT.

Copyright © 2025-2026 SecureBit. All rights reserved.
