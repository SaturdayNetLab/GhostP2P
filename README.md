# 👻 Ghost P2P - Ultimate Secure Transfer

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-stable-green.svg)
![Technology](https://img.shields.io/badge/tech-WebRTC%20%7C%20PeerJS%20%7C%20Tailwind-blueviolet.svg)

**Ghost P2P** is a modern, serverless, and ephemeral file transfer and messaging tool that runs entirely in your browser. It leverages **WebRTC** to establish direct, end-to-end encrypted connections between peers, allowing for secure chats and massive file transfers (up to 10GB+) without any middleman servers storing your data.

It is built as a **Single-File Application**, meaning the entire logic resides in one lightweight HTML file.

<img src="Screenshots/Screenshot 2025-12-05 233727.png" alt="App Preview" width="800">
---

## ✨ Key Features

### 🔒 Security & Privacy
* **End-to-End Encryption:** Uses WebRTC data channels with DTLS/SRTP encryption. No server sees your files or messages.
* **Serverless Architecture:** No database. No logs. Once the tab is closed, all data is wiped from memory (RAM-only).
* **XSS Protection:** Input sanitization via `DOMPurify` prevents malicious code injection.

### 🚀 Performance & Usability
* **Huge File Support:** Smart chunking algorithm (64KB chunks) allows transferring files up to **10 GB** without crashing the browser.
* **Smart Clipboard:** Paste images directly into the chat (`Ctrl+V`) to send screenshots instantly.
* **Transfer Manager:** Floating progress panel with cancellation support for active uploads/downloads.
* **Mobile Ready:** Fully responsive design optimized for desktop and mobile viewports (`100dvh`).

### 🛠 Tools
* **Ephemeral Rooms:** Set optional self-destruct timers (e.g., 10 mins) for hosted rooms.
* **Notifications:** Background alerts for completed transfers or new messages.
* **Markdown Support:** Chat supports rich text formatting.

---

## 🚀 Quick Start

### Option 1: Run Locally
Since Ghost P2P is a single-file application, you don't need Node.js, Python, or Docker.

1.  Download the `index.html` file from this repository.
2.  Open it directly in any modern browser (Chrome, Firefox, Edge, Brave).
3.  Start hosting!

### Option 2: Deploy
You can host this static file anywhere for free:
* **GitHub Pages** (Recommended)
* Netlify / Vercel
* Any web server (Apache/Nginx)

---

## 📖 Usage Guide

### 1. Host a Room
1.  Enter your **Display Name**.
2.  (Optional) Set a **Password** or a **Self-Destruct Timer**.
3.  Click **Create Room**.
4.  Copy the **Room ID** (click on it in the header) or the **Invite Link** and send it to your peer.

### 2. Join a Room
1.  Open the application.
2.  Enter your **Display Name**.
3.  Paste the **Room ID** (or use the invite link).
4.  Enter the password (if set) and click **Connect**.

### 3. Sharing Files
* **Drag & Drop:** Drop files anywhere on the window.
* **Clipboard:** Copy an image and press `Ctrl+V` in the chat input.
* **Attachment:** Click the paperclip icon.

---

## ⚙️ Technical Stack

* **Core:** HTML5, Vanilla JavaScript (ES6+)
* **Networking:** [PeerJS](https://peerjs.com/) (WebRTC wrapper)
* **Styling:** [Tailwind CSS](https://tailwindcss.com/) (CDN)
* **Security:** [DOMPurify](https://github.com/cure53/DOMPurify)
* **Formatting:** [Marked.js](https://github.com/markedjs/marked)

### How it works
1.  **Signaling:** The app connects to a public STUN server (via PeerJS Cloud) only to perform the initial handshake (discover IP/Ports).
2.  **P2P Connection:** Once peers find each other, a direct data tunnel is established.
3.  **Data Flow:** Files and chats flow directly from User A to User B. The server is no longer involved.

---

## 🤝 Contributing

Contributions are welcome! Since this is a single-file project, please ensure:
1.  Keep the code structure clean.
2.  Do not introduce heavy external dependencies unless necessary.
3.  Test large file transfers before submitting a Pull Request.

---

## 📄 License

This project is licensed under the **MIT License** - see the LICENSE file for details.

---

*Made with ❤️ for privacy and speed.*
