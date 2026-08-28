<div align="center">

<img src="logo/securebit-mark.svg" alt="SecureBit.chat" width="110">

# SecureBit Desktop

**Native desktop applications for SecureBit Chat — a zero-server, peer-to-peer messenger with end-to-end encryption.**

[Downloads](#downloads) · [Group chats](#group-chats) · [Installing](#installing) · [Automatic updates](#automatic-updates) · [Verifying your download](#verifying-your-download) · [Security](#security-and-trust) · [Privacy](#privacy)

</div>

---

## Downloads

Current release: **0.5.0** — group chats over a peer-to-peer mesh.

| Platform | Requirements | File |
|---|---|---|
| Windows | Windows 10 (1809+) or 11, x64 | [SecureBit.Chat_0.5.0_x64-setup.exe](https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.5.0/SecureBit.Chat_0.5.0_x64-setup.exe) |
| macOS | macOS 11+, Intel or Apple Silicon | [SecureBit.Chat_0.5.0_x64.dmg](https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.5.0/SecureBit.Chat_0.5.0_x64.dmg) |
| Linux — any distribution | glibc 2.31+ (Ubuntu 20.04+, Debian 11+, Fedora 35+) | [SecureBit.Chat_0.5.0_amd64.AppImage](https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.5.0/SecureBit.Chat_0.5.0_amd64.AppImage) |
| Linux — Debian / Ubuntu | glibc 2.31+, x86_64 | [SecureBit.Chat_0.5.0_amd64.deb](https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.5.0/SecureBit.Chat_0.5.0_amd64.deb) |
| Linux — Fedora / RHEL / openSUSE | glibc 2.31+, x86_64 | [SecureBit.Chat-0.5.0-1.x86_64.rpm](https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.5.0/SecureBit.Chat-0.5.0-1.x86_64.rpm) |

The macOS build is compiled for Intel and runs on Apple Silicon through Rosetta.

All releases are on the [releases page](https://github.com/SecureBitChat/securebit-desktop/releases).

---

## Group chats

New in 0.5.0. A group is a mesh of the same peer-to-peer links the app already uses for 1:1 chats — there is no group server, no group account, and **no shared group key**.

**How a message travels.** Every message is signed by its author and then encrypted *separately for each recipient*, over that recipient's own ratcheted pairwise link. Compromising one member's device gives an attacker that member's links and nothing else; there is no single key whose loss would open the group's traffic. Each pair of members that has no direct connection yet dials one for itself, so the mesh completes without anyone relaying plaintext.

**How you know who is in the room.** Members compare one seven-digit group code, read aloud the same way as the 1:1 verification code. It is built **commit-then-reveal**: everyone publishes the hash of a secret nonce first, and nobody reveals until every commitment has arrived. A member who tries to sit in the middle of two others must fix their commitments before they see a single honest nonce, so they cannot grind the digits into a match — they are reduced to one guess at 10⁻⁷. That ordering is what makes a code short enough to read aloud actually safe.

**Identity.** A group gets its own ECDSA P-384 key pair, generated per group per device, published only over already-verified pairwise channels, and destroyed with the group. The 1:1 protocol is untouched by it.

**Limits and scope.** Up to 8 members — a mesh limit, not a cryptographic one, since pairwise connections grow as N(N−1)/2. Group traffic is text; file transfer, voice messages and calls remain 1:1 features. A desktop member and a browser member can be in the same group: both implementations are tested against each other as members of one group, on the safety code, on every signature, and on the envelope.

---

## Installing

### Windows

1. Run `SecureBit.Chat_0.5.0_x64-setup.exe`.
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

**AppImage** — works on any distribution, nothing to install:

```bash
wget https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.5.0/SecureBit.Chat_0.5.0_amd64.AppImage
chmod +x SecureBit.Chat_0.5.0_amd64.AppImage
./SecureBit.Chat_0.5.0_amd64.AppImage
```

To add it to your application menu:

```bash
sudo mv SecureBit.Chat_0.5.0_amd64.AppImage /opt/securebit-chat.AppImage

cat > ~/.local/share/applications/securebit-chat.desktop <<'EOF'
[Desktop Entry]
Name=SecureBit Chat
Exec=/opt/securebit-chat.AppImage
Type=Application
Categories=Network;InstantMessaging;
EOF
```

**Debian / Ubuntu:**

```bash
sudo apt install ./SecureBit.Chat_0.5.0_amd64.deb
```

**Fedora / RHEL / openSUSE:**

```bash
sudo dnf install ./SecureBit.Chat-0.5.0-1.x86_64.rpm
```

The `.deb` and `.rpm` packages register the app in your menu and pull in the WebKitGTK runtime through your package manager; the AppImage carries its own copy.

---

## Automatic updates

The app checks for a new version shortly after launch, and on demand when you click the version number next to the SecureBit wordmark in the sidebar. When one is available you get a prompt: **Update now** downloads and installs it, then offers to restart.

**Why the update is trustworthy.** Every update bundle is signed with a key that never leaves the maintainer's machine, and the app verifies that signature against a public key compiled into the binary *before* installing anything. A bundle that is unsigned, altered, or signed by any other key is refused. Neither GitHub nor the network is part of the trust model — compromising either does not let anyone push code into your installation.

**Availability**

| Platform | Status |
|---|---|
| macOS | Automatic updates from 0.3.0 onward |
| Windows | Manual — download and run the new installer |
| Linux | Manual — download and replace the AppImage, or install the new `.deb` / `.rpm` |

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
Get-FileHash .\SecureBit.Chat_0.5.0_x64-setup.exe -Algorithm SHA256
```

| File | SHA-256 |
|---|---|
| `SecureBit.Chat_0.5.0_x64-setup.exe` | `17c7e6fed110b7fd4f08b4a434abd6575eff5f97c3fa4f69b98140c9f1d375e5` |
| `SecureBit.Chat_0.5.0_x64.dmg` | `01d602ee9f124f70e5c23a5b9bb8a2d03979776c1f22cdedbdffc4789195f7fd` |
| `SecureBit.Chat_0.5.0_amd64.AppImage` | `f0b60851e27cc38ed3636d7522cdcb06dd525d9b789e60f104370299b2f187ca` |
| `SecureBit.Chat_0.5.0_amd64.deb` | `97c9eb9234b5b60b70625d2b671141140d65356f63680bb48b4b7a510108353f` |
| `SecureBit.Chat-0.5.0-1.x86_64.rpm` | `863ff47135a00a1195b21299b26416fb45ebe9541dc789f37b1b1dd473854976` |

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
- A Double Ratchet over the wire, so every message gets its own key and a compromised device does not open the messages around it
- AES-256-GCM for messages and file chunks, with HMAC-SHA-256 over the payload
- A short verification code that both peers derive independently — comparing it out of band is what rules out an attacker in the middle
- In groups, one signature per message and separate encryption per member, over each member's own ratcheted link — no shared group key exists
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

A group holds a direct connection to every other member, so its bandwidth and CPU cost grow with the number of members — a full eight-member group is comfortable on the recommended specification.

Networking: the app uses WebRTC and needs outbound UDP for STUN (port 3478) and peer traffic. Restrictive firewalls fall back to a TLS relay on port 443.

---

## Status and roadmap

The desktop applications are in public beta. The cryptographic core is production-ready and shared with the web client; what is still being refined is platform integration and UI polish.

Planned:

- Automatic updates on Windows and Linux
- Distribution through the Microsoft Store, Mac App Store and Snap Store
- Mobile applications
- Larger groups, and group file transfer
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
