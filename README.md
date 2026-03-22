# GhostP2P v2 — Ephemeral Encrypted Chat

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-stable-green.svg)]()
[![Tech](https://img.shields.io/badge/tech-WebRTC%20%7C%20Web%20Crypto%20%7C%20PeerJS-blueviolet.svg)]()
[![Single File](https://img.shields.io/badge/architecture-single%20file-orange.svg)]()

**GhostP2P** is a serverless, ephemeral, end-to-end encrypted chat and file transfer tool that runs entirely in your browser. It uses **WebRTC** for direct peer-to-peer connections and the **Web Crypto API** for application-layer encryption. No servers store your data — close the tab and everything is gone.

Built as a **single HTML file**. No build step, no dependencies to install, no backend.

<p align="center">
  <img src="Screenshots/Screenshot 2026-03-22 160959.png" width="100%" alt="Lobby View">
</p>
<p align="center">
  <img src="Screenshots/Screenshot 2026-03-22 161044.png" width="49%" alt="Chat View">
  <img src="Screenshots/Screenshot 2026-03-22 161057.png" width="49%" alt="Security Info">
</p>

---

## Features

### Security & Encryption

- **ECDH P-256 Key Exchange** — Ephemeral keypairs generated per session. Every connection gets unique shared secrets with forward secrecy.
- **AES-256-GCM Encryption** — Application-layer encryption on top of WebRTC DTLS. All messages, files, images, and voice messages are encrypted before transmission.
- **PBKDF2 Password Protection** — Room passwords are derived using 100,000 iterations with a random salt. Passwords never leave your device in plaintext.
- **Safety Number Verification** — After connecting, both peers see an identical Safety Number (SHA-256 hash of both public keys). Compare it out-of-band to verify no man-in-the-middle attack is occurring.
- **RAM Only** — Zero disk persistence. Close the tab and all data — keys, messages, files — is wiped from memory.
- **XSS Protection** — All user-generated content is sanitized through DOMPurify before rendering.
- **Serverless P2P** — A public STUN server is used only for the initial WebRTC handshake. After that, all data flows directly between peers.

### Chat

- Markdown support (bold, italic, code blocks, links, lists)
- Typing indicators
- Message grouping for consecutive messages
- Desktop notifications for background messages
- Distinct sound effects for sent, received, join, and leave events

### Voice Messages

- Record and send encrypted voice messages
- Live waveform visualization during recording
- In-chat audio player with play/pause and duration display
- Echo cancellation and noise suppression

### Image Sharing

- Inline image preview directly in the chat
- Fullscreen lightbox on click
- Clipboard paste with `Ctrl+V` for instant screenshot sharing
- Drag & drop support

### File Transfer

- Transfers up to 10 GB with 64KB chunking
- Floating progress cards with real-time percentage
- Cancel active transfers at any time
- File type icons for PDF, Word, Excel, video, audio, and more

### Room Management

- Optional password-protected rooms (PBKDF2-derived authentication)
- Self-destruct timer (10 min, 30 min, 1 hour)
- QR code room invites
- One-click invite link copy
- Member sidebar with color-coded avatars
- System messages when peers join or leave

### UI/UX

- Modern dark glassmorphism design with ambient light effects
- Fully responsive — desktop and mobile (`100dvh`)
- Smooth animations and transitions
- Toast notifications

---

## Quick Start

### Option 1: Open Directly

1. Download `index.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Brave, Safari)
3. Done

> **Note:** For two devices to connect, the file must be served over HTTPS (required by WebRTC). Opening locally works for testing on the same machine.

### Option 2: Deploy

Host the single HTML file on any static hosting:

- **GitHub Pages** (recommended — free HTTPS)
- Netlify / Vercel / Cloudflare Pages
- Any web server (Nginx, Apache, Caddy)

```bash
# GitHub Pages example
git add index.html
git commit -m "Deploy GhostP2P v2"
git push origin main
# Enable GitHub Pages in repo settings → Source: main branch
```

---

## Usage

### Host a Room

1. Enter your display name
2. Optionally set a password and/or self-destruct timer
3. Click **Create Room**
4. Share the Room ID — click it to copy, or use the link/QR buttons in the topbar

### Join a Room

1. Enter your display name
2. Paste the Room ID (or use an invite link — it auto-fills)
3. Enter the password if required
4. Click **Connect**

### Verify Encryption

After connecting, a **Safety Number** appears in the chat. Both peers see the same number. Compare it via a separate channel (phone call, in person). If the numbers match, your connection is verified and no one is intercepting it.

### Send Files & Images

- **Drag & Drop** — Drop files anywhere on the chat window
- **Clipboard** — Copy an image and press `Ctrl+V`
- **Attach** — Click the paperclip button to select a file

### Send Voice Messages

1. Click the microphone button
2. Record your message (live waveform + timer shown)
3. Click send to deliver, or trash to cancel

---

## Keyboard Shortcuts

| Action | Shortcut |
|---|---|
| Send message | `Enter` |
| New line | `Shift + Enter` |
| Paste image | `Ctrl + V` |

---

## Technical Stack

| Component | Technology |
|---|---|
| Networking | [PeerJS](https://peerjs.com/) 1.5.2 (WebRTC wrapper) |
| Key Exchange | ECDH P-256 (Web Crypto API) |
| Encryption | AES-256-GCM (Web Crypto API) |
| Password Hashing | PBKDF2 SHA-256, 100k iterations |
| XSS Protection | [DOMPurify](https://github.com/cure53/DOMPurify) 3.0.6 |
| Markdown | [Marked.js](https://github.com/markedjs/marked) 15.0.7 |
| Icons | [Font Awesome](https://fontawesome.com/) 6.5.1 |
| Fonts | Outfit + JetBrains Mono (Google Fonts) |

### How It Works

```
1. SIGNALING     → PeerJS Cloud (STUN) discovers IP/ports
2. KEY EXCHANGE  → ECDH P-256 ephemeral keypairs → shared secret
3. ENCRYPTION    → AES-256-GCM encrypts all data on application layer
4. P2P TUNNEL    → WebRTC DataChannel carries encrypted payloads
5. TAB CLOSE     → All keys, messages, files wiped from RAM
```

The STUN server only sees that two peers want to connect (IP metadata). It never sees message content, file content, or encryption keys.

---

## 🔐 Security Model

### What GhostP2P Protects Against

- Message interception — encrypted in transit with AES-256-GCM
- Server-side data storage — no server stores anything
- XSS attacks — DOMPurify sanitization on all rendered content
- Weak room passwords — PBKDF2 with 100k iterations
- Man-in-the-middle attacks — Safety Number verification
- Session persistence — RAM-only, ephemeral keys per session

### Known Limitations

- **Metadata** — The PeerJS signaling server sees that Peer A connects to Peer B (IP addresses), but not what they communicate.
- **STUN Servers** — Google's STUN servers see both peers' IP addresses during the initial handshake.
- **CDN Trust** — External libraries are loaded from CDNs. All are pinned to exact versions to minimize supply-chain risk.
- **Browser Trust** — Security relies on the browser's Web Crypto API implementation being correct.

---

## Contributing

Contributions are welcome. Since this is a single-file project:

1. Keep everything in one HTML file
2. Don't add heavy external dependencies
3. Test with at least 2 peers before submitting a PR
4. Test file transfers (small + large)
5. Test on mobile

---

## License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

*Made with ❤️ for privacy and speed.*
