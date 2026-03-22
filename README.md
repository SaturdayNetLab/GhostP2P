# 👻 GhostP2P v2 — Ephemeral Encrypted Chat

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-stable-green.svg)]()
[![Tech](https://img.shields.io/badge/tech-WebRTC%20%7C%20Web%20Crypto%20%7C%20PeerJS-blueviolet.svg)]()
[![Single File](https://img.shields.io/badge/architecture-single%20file-orange.svg)]()

**GhostP2P** is a serverless, ephemeral, end-to-end encrypted chat and file transfer tool that runs entirely in your browser. It uses **WebRTC** for direct peer-to-peer connections and the **Web Crypto API** for application-layer encryption. No servers store your data — close the tab and everything is gone.

Built as a **single HTML file**. No build step, no dependencies to install, no backend.

---

## ✨ Features

### 🔒 Security & Encryption

- **ECDH P-256 Key Exchange** — Ephemeral keypairs generated per session. Every connection gets unique shared secrets with forward secrecy.
- **AES-256-GCM Encryption** — Application-layer encryption on top of WebRTC DTLS. All messages, files, images, and voice messages are encrypted before transmission.
- **PBKDF2 Password Protection** — Room passwords are derived using 100,000 iterations with a random salt. Passwords never leave your device in plaintext.
- **Safety Number Verification** — After connecting, both peers see an identical Safety Number (SHA-256 hash of both public keys). Compare it out-of-band to verify no man-in-the-middle attack is occurring.
- **RAM Only** — Zero disk persistence. Close the tab and all data — keys, messages, files — is wiped from memory.
- **XSS Protection** — All user-generated content is sanitized through DOMPurify before rendering.
- **Serverless P2P** — A public STUN server is used only for the initial WebRTC handshake. After that, all data flows directly between peers.

### 💬 Chat

- **Markdown Support** — Bold, italic, code blocks, links, lists.
- **Typing Indicators** — See when your peer is typing.
- **Message Grouping** — Consecutive messages from the same sender are visually grouped.
- **Desktop Notifications** — Get notified of new messages when the tab is in the background.
- **Sound Effects** — Distinct sounds for sent, received, join, and leave events.

### 🎤 Voice Messages

- **Record & Send** — Click the microphone button to record encrypted voice messages.
- **Live Waveform** — Real-time audio visualization during recording.
- **In-Chat Player** — Play/pause voice messages directly in the chat with waveform display and duration.
- **Echo Cancellation & Noise Suppression** — Clean audio capture via browser APIs.

### 🖼️ Image Sharing

- **Inline Preview** — Images are displayed as thumbnails directly in the chat.
- **Lightbox** — Click any image to view it fullscreen.
- **Clipboard Paste** — Press `Ctrl+V` to paste and send screenshots instantly.
- **Drag & Drop** — Drop image files anywhere on the window.

### 📁 File Transfer

- **Up to 10 GB** — Smart 64KB chunking algorithm for large file transfers.
- **Progress Tracking** — Floating transfer cards with real-time progress bars.
- **Cancellation** — Cancel active transfers at any time.
- **File Type Icons** — Visual icons for PDF, Word, Excel, video, audio, and more.

### 🛠️ Room Management

- **Password-Protected Rooms** — Optional room passwords with PBKDF2-derived authentication.
- **Self-Destruct Timer** — Set rooms to auto-close after 10 min, 30 min, or 1 hour.
- **QR Code Invites** — Share rooms via a visual QR code.
- **Invite Links** — One-click copy of the full invite URL.
- **Member Sidebar** — See all connected peers with color-coded avatars.
- **Disconnect Detection** — System messages when peers join or leave.

### 🎨 UI/UX

- **Glassmorphism Design** — Modern dark UI with ambient light orbs and noise texture.
- **Fully Responsive** — Optimized for desktop and mobile (`100dvh`).
- **Animated Transitions** — Smooth fade-in, slide, and scale animations.
- **Toast Notifications** — Non-intrusive status messages.

---

## 🚀 Quick Start

### Option 1: Open Directly

1. Download `index.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Brave, Safari)
3. Done — no server needed for local testing

> **Note:** For two devices to connect, the file must be served over HTTPS (required by WebRTC). Opening locally works for testing on the same machine.

### Option 2: Deploy to Any Web Server

Host the single HTML file on any static hosting:

- **GitHub Pages** (recommended — free HTTPS)
- Netlify / Vercel / Cloudflare Pages
- Any web server (Nginx, Apache, Caddy)

```bash
# Example: GitHub Pages
git add index.html
git commit -m "Deploy GhostP2P v2"
git push origin main
# Enable GitHub Pages in repo settings → Source: main branch
```

---

## 📖 Usage

### Host a Room

1. Enter your **display name**
2. (Optional) Set a **password** and/or **self-destruct timer**
3. Click **Create Room**
4. Share the **Room ID** — click it to copy, or use the 🔗 (link) or 📱 (QR) buttons

### Join a Room

1. Enter your **display name**
2. Paste the **Room ID** (or use an invite link — it auto-fills)
3. Enter the **password** if one was set
4. Click **Connect**

### Verify Encryption

After connecting, a **Safety Number** appears in the chat. Both you and your peer will see the same number. Compare it via phone call, SMS, or in person. If the numbers match, your connection is verified — no one is intercepting it.

### Send Files & Images

- **Drag & Drop** — Drop files anywhere on the chat window
- **Clipboard** — Copy an image and press `Ctrl+V`
- **Attach** — Click the 📎 button to select a file

### Send Voice Messages

1. Click the 🎤 (microphone) button
2. Record your message — a live waveform and timer are shown
3. Click the ✅ (send) button to send, or 🗑️ (trash) to cancel

---

## ⌨️ Keyboard Shortcuts

| Action | Shortcut |
|---|---|
| Send message | `Enter` |
| New line | `Shift + Enter` |
| Paste image | `Ctrl + V` |

---

## 🔧 Technical Stack

| Component | Technology |
|---|---|
| Networking | [PeerJS](https://peerjs.com/) (WebRTC wrapper) |
| Key Exchange | ECDH P-256 (Web Crypto API) |
| Encryption | AES-256-GCM (Web Crypto API) |
| Password Hashing | PBKDF2 SHA-256, 100k iterations |
| XSS Protection | [DOMPurify](https://github.com/cure53/DOMPurify) 3.0.6 |
| Markdown | [Marked.js](https://github.com/markedjs/marked) 15.0.7 |
| Icons | [Font Awesome](https://fontawesome.com/) 6.5.1 |
| Fonts | Outfit + JetBrains Mono (Google Fonts) |
| UI | Vanilla CSS with CSS Variables, Glassmorphism |

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

✅ Message interception (encrypted in transit)
✅ Server-side data storage (no server stores anything)
✅ XSS attacks (DOMPurify sanitization)
✅ Weak room passwords (PBKDF2 100k iterations)
✅ Man-in-the-middle attacks (Safety Number verification)
✅ Session persistence (RAM-only, ephemeral keys)

### Known Limitations

⚠️ **Metadata** — The PeerJS signaling server sees that Peer A connects to Peer B (IP addresses), but not what they communicate.
⚠️ **STUN Servers** — Google's STUN servers see both peers' IP addresses during the initial handshake.
⚠️ **CDN Trust** — External libraries (PeerJS, DOMPurify, Marked, Font Awesome) are loaded from CDNs. If a CDN is compromised, malicious code could be injected. Pin versions are used to minimize this risk.
⚠️ **Browser Trust** — Security relies on the browser's Web Crypto API implementation being correct.

---

## 🤝 Contributing

Contributions are welcome! Since this is a single-file project:

1. Keep everything in one HTML file
2. Don't add heavy external dependencies
3. Test with at least 2 peers before submitting a PR
4. Test file transfers (small + large files)
5. Test on mobile

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

*Made with ❤️ for privacy and speed.*
