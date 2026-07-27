# SecureBit Desktop Applications

<div align="center">

<img src="logo/securebit-mark.svg" alt="SecureBit.chat" width="120">

**Official desktop installers for SecureBit Chat - the world's most secure P2P messenger**

[![Core Repository](https://img.shields.io/badge/Core-Open%20Source-blue?style=for-the-badge)](https://github.com/SecureBitChat/securebit-core)
[![Web Version](https://img.shields.io/badge/Web-Open%20Source-blue?style=for-the-badge)](https://github.com/SecureBitChat/securebit-chat)
[![License](https://img.shields.io/badge/License-Proprietary-red?style=for-the-badge)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey?style=for-the-badge)](/)

</div>

---

## Downloads

### Windows

| Version | Architecture | Download |
|---------|--------------|----------|
| **Windows 10/11** | x64 (NSIS Installer) | [SecureBit.Chat_0.1.0_x64-setup.exe](https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.1.0/SecureBit.Chat_0.1.0_x64-setup.exe) |

###  macOS

| Version | Architecture | Download |
|---------|--------------|----------|
| **macOS 11+** | Intel x64 (runs on Apple Silicon via Rosetta) | [SecureBit.Chat_0.3.0_x64.dmg](https://github.com/SecureBitChat/securebit-desktop/releases/latest/download/SecureBit.Chat_0.3.0_x64.dmg) |

###  Linux

| Distribution | Format | Download |
|--------------|--------|----------|
| **Universal Linux** | AppImage | [SecureBit Chat_0.1.0_amd64.AppImage](https://github.com/SecureBitChat/securebit-desktop/releases/download/v0.1.0/SecureBit.Chat_0.1.0_amd64.AppImage) |

> **Latest release**: macOS `0.3.0` · Windows and Linux `0.1.0` (rebuilt separately)
>
> ** Coming in Q1 2026**: Official distribution via Windows Store, Mac App Store, and Snap Store

---

## Automatic Updates

From **0.3.0** onwards macOS builds update themselves — there is no need to come
back here and download a new disk image for every release.

The app checks for a new version shortly after launch, and you can check on demand
by clicking the version number next to the SecureBit wordmark in the sidebar. When
one is found, a prompt appears: **Update now** downloads and installs it, then
offers to restart.

**How the update is trusted.** Every update bundle is signed with an offline key
that never leaves the maintainer's machine, and the app verifies that signature
against a public key compiled into the binary *before* installing anything. An
update that is unsigned, tampered with, or signed by anyone else is refused. That
means neither GitHub nor the network can push code into your installation — the
distribution channel is not part of the trust model.

> **Coming from 0.1.0?** That build predates the updater, so download 0.3.0 once
> from the table above. Updates after that are in-app.
>
> **Windows and Linux** are still on 0.1.0 and update manually for now; automatic
> updates reach them with their next build.

---

##  Security & Trust

### Why Trust SecureBit Desktop?

SecureBit Desktop applications are built on a **transparent security architecture** that separates security-critical code from platform-specific UI:

```
┌─────────────────────────────────────────┐
│   Desktop Application (Proprietary)    │
│   • Native UI Components                │
│   • Platform Integrations               │
│   • System Notifications                │
│   • Auto-update System                  │
└──────────────┬──────────────────────────┘
               │ Uses
               ▼
┌─────────────────────────────────────────┐
│    SecureBit Core (Open Source)         │
│   • All Cryptographic Operations        │
│   • P2P Protocol Implementation         │
│   • Key Exchange & Verification         │
│   • End-to-End Encryption               │
│   • WebRTC Security Layer               │
└─────────────────────────────────────────┘
```

### Open Source Core

The **entire security foundation** is open source and auditable:

#### [securebit-core](https://github.com/SecureBitChat/securebit-core)
Complete cryptographic engine with:
- ECDH key exchange implementation
- DTLS security layer
- SAS verification protocol
- ASN.1 validation
- WebRTC security primitives
- All encryption/decryption logic
- Quantum-resistant cryptography roadmap

#### [securebit-chat](https://github.com/SecureBitChat/securebit-chat)
Full web version source code:
- Complete UI implementation
- User interface patterns
- State management
- P2P connection logic
- All application features

###  Desktop Application Architecture

**What's Proprietary (UI Layer)**:
- Platform-specific native UI components (Windows/macOS/Linux)
- System integration code (menu bars, notifications, file pickers)
- Auto-update infrastructure and delivery system
- Platform-specific performance optimizations
- Branding and design assets

**What's Open Source (Security Layer)**:
- **100% of cryptographic operations**
- **100% of protocol implementation**
- **100% of key exchange and verification**
- **100% of encryption/decryption**
- **All security-critical components**

###  Why This Approach?

This architecture follows industry best practices used by:

| Project | Model | Example |
|---------|-------|---------|
| **Telegram** | Open protocol, closed desktop UI | Desktop apps proprietary, protocol open |
| **Signal** | Open protocol, desktop wrapper | Core OSS, desktop builds proprietary elements |
| **Visual Studio Code** | Open core, proprietary builds | OSS core, Microsoft builds with additions |
| **Proton Mail** | Open crypto, proprietary UI | Crypto libs open, apps closed |

**Benefits of this approach**:
1. **Full Security Auditability** - All crypto code can be reviewed
2. **Business Sustainability** - Platform development requires resources
3. **Better User Experience** - Native integrations improve UX
4. **Faster Development** - Focus resources on security and features
5. **Trust Through Transparency** - Critical code is verifiable

###  What Security Researchers Can Audit

You have **complete access** to audit:

```bash
# Clone the cryptographic core
git clone https://github.com/SecureBitChat/securebit-core.git

# Review all security-critical code
cd securebit-core
grep -r "encrypt\|decrypt\|key\|signature\|verify" src/

# Run the test suite
npm install
npm test

# Build and test locally
npm run build
```

**Available for auditing**:
- Full cryptographic implementation
- Protocol specification and documentation
- Key exchange mechanisms
- Encryption algorithms and parameters
- Authentication and verification logic
- Network security layer (WebRTC/DTLS)
- All security-relevant APIs

**Not needed for security auditing**:
- Platform UI rendering code
- Window management logic
- System notification formatting
- Auto-update download mechanism
- Theme and styling systems

> **Key Principle**: If it touches your messages, keys, or connections - it's open source. If it draws buttons or manages windows - it's not security-critical.

### Verification & Signatures

All releases are cryptographically signed and verifiable:

#### Windows
```powershell
# Verify digital signature
Get-AuthenticodeSignature ".\SecureBit Chat_0.1.0_x64-setup.exe"

# Expected output:
# SignerCertificate: CN=SecureBit Inc.
# Status: Valid
```

#### macOS
```bash
# Verify code signature
codesign -dvv "/Applications/tauri-webrtc-chat.app"

# Check notarization
spctl -a -vvv -t install "/Applications/tauri-webrtc-chat.app"

# Expected: "accepted"
```

#### Linux
```bash
# Verify AppImage signature (if provided)
gpg --verify "SecureBit Chat_0.1.0_amd64.AppImage.sig" "SecureBit Chat_0.1.0_amd64.AppImage"

# Verify SHA256 checksum
sha256sum -c SHA256SUMS.txt
```

## Installation

### Windows Installation

1. **Download** `SecureBit Chat_0.1.0_x64-setup.exe`
2. **Run** the NSIS installer
3. **Follow** the installation wizard
4. **Launch** from Start Menu or Desktop shortcut

**Installation Location**: `C:\Program Files\SecureBit Chat\`

**First Launch**:
- Windows may show SmartScreen warning (click "More info" → "Run anyway")
- Grant permissions for network access if prompted
- Application will check for updates automatically

### macOS Installation

1. **Download** `tauri-webrtc-chat.app.zip`
2. **Unzip** the archive (double-click or use `unzip` command)
3. **Drag** `tauri-webrtc-chat.app` to Applications folder
4. **First launch**: Right-click app → "Open" (bypasses Gatekeeper)

**Installation Location**: `/Applications/tauri-webrtc-chat.app`

**First Launch**:
- macOS may show "unidentified developer" warning
- Right-click → Open (first time only)
- Grant microphone/camera permissions if needed
- Grant network access if prompted

**Troubleshooting**:
```bash
# If app won't open, remove quarantine attribute
xattr -d com.apple.quarantine /Applications/tauri-webrtc-chat.app

# Or allow in Security & Privacy settings
# System Preferences → Security & Privacy → General → "Open Anyway"
```

### Linux Installation

#### AppImage (Universal - Recommended)
```bash
# Download
wget https://github.com/SecureBitChat/securebit-desktop/releases/latest/download/SecureBit.Chat_0.1.0_amd64.AppImage

# Make executable
chmod +x SecureBit\ Chat_0.1.0_amd64.AppImage

# Run
./SecureBit\ Chat_0.1.0_amd64.AppImage

# Optional: Integrate with system (creates desktop entry)
./SecureBit\ Chat_0.1.0_amd64.AppImage --appimage-extract
sudo mv squashfs-root /opt/securebit-chat
sudo ln -s /opt/securebit-chat/AppRun /usr/local/bin/securebit-chat
```

**Alternative: Run directly without extraction**
```bash
# Move to a permanent location
sudo mv "SecureBit Chat_0.1.0_amd64.AppImage" /opt/securebit-chat.AppImage

# Create desktop entry
cat > ~/.local/share/applications/securebit-chat.desktop << EOF
[Desktop Entry]
Name=SecureBit Chat
Exec=/opt/securebit-chat.AppImage
Icon=securebit-chat
Type=Application
Categories=Network;InstantMessaging;
EOF
```

**Installation Location**: User's choice (AppImage is portable)

---

## Distribution Channels

| Channel | Status | Availability | Updates | Notes |
|---------|--------|--------------|---------|-------|
| **GitHub Releases** | ✅ Available Now | Immediate | Manual/Auto | Direct download, latest features |
| **Windows Store** | 🔄 Coming Q1 2026 | After review | Automatic | Official Microsoft distribution |
| **Mac App Store** | 🔄 Coming Q1 2026 | After review | Automatic | Official Apple distribution |
| **Snap Store** | 🔄 Coming Q1 2026 | After review | Automatic | Linux universal package |
| **Flathub** | 🔄 Coming Q2 2026 | After review | Automatic | Linux sandboxed distribution |


##  System Requirements

### Minimum Requirements

| Platform | OS Version | RAM | Disk Space | Additional |
|----------|-----------|-----|------------|------------|
| **Windows** | Windows 10 (1809+) or 11 | 4 GB | 200 MB | Internet connection |
| **macOS** | macOS 11 Big Sur or later | 4 GB | 200 MB | Internet connection |
| **Linux** | Ubuntu 20.04+ / Debian 11+ / Fedora 35+ | 4 GB | 200 MB | GLIBC 2.31+, Internet |

### Recommended Requirements

| Platform | OS Version | RAM | Disk Space | Additional |
|----------|-----------|-----|------------|------------|
| **Windows** | Windows 11 | 8 GB | 500 MB | Webcam + Microphone |
| **macOS** | macOS 13 Ventura or later | 8 GB | 500 MB | Webcam + Microphone |
| **Linux** | Ubuntu 22.04+ / Fedora 38+ | 8 GB | 500 MB | Webcam + Microphone |

### Network Requirements

- **Ports**: UDP 3478 (STUN), UDP 19302-19309 (WebRTC)
- **Firewall**: Allow P2P connections (WebRTC)
- **NAT**: Most NAT types supported via STUN/TURN
- **Bandwidth**: 128 kbps minimum for audio, 1 Mbps for video

---

### Technical Documentation

- **[Security Documentation](https://github.com/SecureBitChat/securebit-core/blob/main/SECURITY.md)** - Cryptographic implementation details
- **[Protocol Specification](https://github.com/SecureBitChat/securebit-core/blob/main/docs/PROTOCOL.md)** - Technical protocol documentation
- **[Architecture](https://github.com/SecureBitChat/securebit-core/blob/main/docs/ARCHITECTURE.md)** - System design and components
- **[API Reference](https://github.com/SecureBitChat/securebit-core/blob/main/docs/API.md)** - Core API documentation


### Get Help

**For questions and support**:
- **Community Forum**: [GitHub Discussions](https://github.com/SecureBitChat/securebit-desktop/discussions)
- **Email**: support@securebit.chat

---

## Contributing

While the desktop application source code is not publicly available, there are many ways to contribute to SecureBit:

### Contribute to Open Source Core

#### [securebit-core](https://github.com/SecureBitChat/securebit-core)
The cryptographic engine and protocol implementation:
- Improve encryption algorithms
- Enhance protocol security
- Fix bugs in core components
- Add new security features
- Write tests and documentation

#### [securebit-chat](https://github.com/SecureBitChat/securebit-chat)
The web version with full source code:
- Improve user interface
- Add new features
- Fix bugs
- Enhance performance
- Write tests

---

## License

### Desktop Applications
- **License**: Proprietary
- **Usage**: Free for personal and commercial use
- **Restrictions**: No reverse engineering, redistribution requires permission
- **Terms**: See [LICENSE](LICENSE) for full terms

---

## ⚠️ Beta Notice

SecureBit Desktop applications are currently in **public beta** (v0.1.x). While the core cryptographic components are production-ready and battle-tested, the desktop wrapper applications are being actively refined.

**What this means**:
- Security features are fully functional and audited
- Core messaging features work reliably
- UI polish and platform integration improvements ongoing
- Some edge cases and minor bugs may exist
- Performance optimizations in progress

---

## Privacy & Data

### What We Collect: Nothing

SecureBit is designed with **zero data collection**:

- ❌ No analytics or telemetry
- ❌ No usage statistics
- ❌ No crash reports (unless you opt-in)
- ❌ No personal information
- ❌ No message metadata
- ❌ No contact lists stored on servers

### What We Store: Nothing

- ❌ No messages stored on servers
- ❌ No user accounts on servers
- ❌ No connection logs
- ❌ No IP addresses logged
- ❌ No encryption keys stored

---

## Roadmap

### Q1 2026
- Desktop beta releases
- Windows Store submission
- Mac App Store submission
- Snap Store submission
- Group chat support
- File transfer improvements

### Q2 2026
- Mobile apps (iOS/Android)
- Hardware security key support
- Multi-device sync
- Themes and customization
- Advanced statistics

### Q3 2026
- Quantum-resistant cryptography
- Message search improvements
- Video call enhancements

Vote on features in [Discussions](https://github.com/SecureBitChat/securebit-desktop/discussions).

---

## Support the Project

### Ways to Support

- ⭐ **Star the repositories** on GitHub
- 🐛 **Report bugs** and test beta features
- 📢 **Spread the word** to friends and colleagues

### Enterprise Support

Need help deploying SecureBit in your organization?
- Custom deployment support
- Training and onboarding
- Priority support
- Custom features


---


<div align="center">

**🔒 Built with privacy in mind by the SecureBit Team**

[⬆ Back to Top](#securebit-desktop-applications)

---

Made with ❤️ for privacy advocates worldwide

Copyright © 2025-2026 SecureBit Inc. All rights reserved.

</div>
