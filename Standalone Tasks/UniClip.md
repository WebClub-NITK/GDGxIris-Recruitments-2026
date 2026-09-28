# Task ID: UniClip

`Full Stack Web Development`,  `Real-Time Sync`

**Mentor:** [Pari Tibrewal](https://github.com/pari1011) | [8240188219](https://wa.me/918240188219)

**Difficulty:** Medium

## Description

Build a web application that allows users to sync text and links across their devices in real time. Users can create a temporary session, pair devices, and share clipboard contents without creating an account.

**Example:** Copy a link on your phone, tap **“Sync Clipboard”**, and then copy it from your laptop.

## Features to Implement

### 1. Session & Pairing
- Create a temporary session.
- Join using a short pairing code.
- No login/registration.
- Show connected devices.

### 2. Clipboard Sync
- Read/send text using the Clipboard API.
- Provide a **“Sync Clipboard”** action.
- Support text and links.
- Detect and display links separately.
- Handle clipboard permission errors gracefully.

> **Note:** Browser restrictions prevent continuous background clipboard monitoring in many cases, so the core implementation should use an explicit user action.

### 3. Real-Time Synchronization
- New entries should appear without refreshing the page.
- Maintain a consistent order of entries.
- Avoid duplicate entries.

### 4. Clipboard History
- Show recent synced items.
- Display content, device, and time.
- Allow users to copy an old entry back to their clipboard.
- Delete individual entries or clear all history.


### 5. Reconnection
- Handle temporary connection loss gracefully.
- Reconnect automatically.
- Avoid duplicate entries after reconnecting.

## Bonus

**Any 2 bonus features make the task Hard.**

1. **QR Pairing** — Join a session by scanning a QR code.
2. **Image Sync** — Sync small images/screenshots.
3. **End-to-End Encryption** — The server should not be able to read the actual clipboard content.
4. **Burn After Sync** — Remove an item from shared history after it has been successfully synced to a device.
5. **Automatic Clipboard Detection** — Detect clipboard changes automatically where browser permissions allow it, with a manual fallback.

> **Note:** Automatic clipboard monitoring is subject to browser security and permission restrictions.

## Deliverables

1. **Source Code**
   - Frontend and backend source code.
   - Public GitHub repository.
   - Include an appropriate `.gitignore`.

2. **README**
   - Setup instructions.
   - Dependencies.
   - Environment variables/configuration.
   - How to run the project.
   - Screenshots.
   - Brief explanation of the architecture.

3. **Demo Video**
   - Recommended length: 3–5 minutes.
   - Ideally demonstrate syncing between two devices or browser windows.

4. **Deployed Link**
   - Provide a working deployed version of the application.

## Resources

- [Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)
- [WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API)
- [Firebase Realtime Database](https://firebase.google.com/docs/database)
- [Supabase Realtime](https://supabase.com/docs/guides/realtime)
- [qrcode.react](https://www.npmjs.com/package/qrcode.react)
