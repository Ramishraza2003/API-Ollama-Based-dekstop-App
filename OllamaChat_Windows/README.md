<div align="center">

# 💬 Local Chat — Flutter + Ollama

**A private, offline-first chat app powered entirely by your own local LLM.**

No cloud API. No API key. No telemetry. Nothing ever leaves your machine or network.

[![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B?logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.x-0175C2?logo=dart&logoColor=white)](https://dart.dev)
[![Ollama](https://img.shields.io/badge/LLM-Ollama-000000?logo=ollama&logoColor=white)](https://ollama.com)
[![Platform](https://img.shields.io/badge/Platform-Android%20%7C%20iOS%20%7C%20macOS%20%7C%20Desktop-blue)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

</div>

---

## Overview

**Local Chat** is a clean, light-themed chat client that talks exclusively to a **local Ollama server**. It's built for people who want a ChatGPT-style experience without sending a single token to the cloud — everything is streamed, parsed, and rendered on-device.

The raw Ollama streaming/JSON plumbing is never exposed to the user — the UI only ever shows finished, Markdown-rendered chat text, with a subtle animated typing indicator while a reply is being generated.

## ✨ Features

- 🔒 **100% local** — every request goes to `http://<your-ollama-host>:11434`, never to a third-party cloud
- ⚡ **Streaming responses** — tokens are consumed line-by-line as newline-delimited JSON and rendered live
- 📝 **Markdown rendering** — code blocks, lists, and formatting render correctly via `flutter_markdown`
- 🎛️ **Model switcher** — dropdown lists whatever models you've pulled locally (`ollama pull ...`)
- ⚙️ **Configurable host** — point the app at localhost, an emulator alias, or any LAN IP from Settings
- ✅ **Connection test** — verify Ollama reachability before you start chatting
- 🎨 **Polished light UI** — indigo/coral accents, rounded bubbles, Inter font, animated typing dots
- 📱 **Cross-platform** — Android, iOS, macOS, and desktop from one codebase

## 📸 Screenshots

<div align="center">
  <i>Add screenshots or a screen recording here — a chat screen and the settings screen work well.</i>
  <br><br>
  <code>docs/screenshot-chat.png</code> &nbsp;•&nbsp; <code>docs/screenshot-settings.png</code>
</div>

## 🏗️ Project Structure

```
lib/
├── main.dart                     # App entry point
├── theme.dart                    # Light theme (colors, fonts, input styling)
├── models/
│   └── chat_message.dart         # Message model
├── services/
│   └── ollama_service.dart       # All Ollama HTTP/streaming logic
├── screens/
│   ├── chat_screen.dart          # Main chat UI
│   └── settings_screen.dart      # Host/model config + connection test
└── widgets/
    └── chat_bubble.dart          # Bubble UI + animated typing dots
pubspec.yaml
analysis_options.yaml
```

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| UI Framework | Flutter (Dart) |
| LLM Runtime | [Ollama](https://ollama.com) (local inference) |
| Networking | `http` package, streamed NDJSON parsing |
| Markdown rendering | `flutter_markdown` |
| Fonts | `google_fonts` (Inter) |

## 🚀 Getting Started

### Prerequisites

- [Flutter SDK](https://docs.flutter.dev/get-started/install) installed and on your `PATH`
- [Ollama](https://ollama.com/download) installed
- A pulled model, e.g. `llama3.2`

### 1. Scaffold the platform folders

This repo ships as a pure Dart/Flutter source tree — the platform folders (`android/`, `ios/`, `macos/`, etc.) are generated on your machine, not committed here:

```bash
flutter create ollama_chat
```

Copy `lib/`, `pubspec.yaml`, and `analysis_options.yaml` from this repo into the generated project, overwriting the defaults, then:

```bash
cd ollama_chat
flutter pub get
```

### 2. Allow local (cleartext) HTTP

Ollama serves plain HTTP on your machine, so each platform needs a small permission tweak — HTTPS-only policies block `http://localhost` by default.

**Android** — `android/app/src/main/AndroidManifest.xml`:
```xml
<manifest ...>
    <uses-permission android:name="android.permission.INTERNET"/>
    <application
        android:usesCleartextTraffic="true"
        ...>
```

**iOS** — `ios/Runner/Info.plist`, inside the top-level `<dict>`:
```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

**macOS** — `macos/Runner/*.entitlements` (both Debug and Release files):
```xml
<key>com.apple.security.network.client</key>
<true/>
```

### 3. Run Ollama

```bash
ollama serve
ollama pull llama3.2   # or any model you like
```

### 4. Point the app at Ollama

| Scenario | Host to use |
|---|---|
| Desktop app / same machine | `http://localhost:11434` (default) |
| Android emulator | `http://10.0.2.2:11434` |
| Real phone/tablet | Your computer's LAN IP, e.g. `http://192.168.1.20:11434` |

For real-device access, make sure the phone is on the same Wi-Fi and set `OLLAMA_HOST=0.0.0.0` before `ollama serve` so it accepts non-localhost connections. Configure this in the app's **Settings** screen.

### 5. Run the app

```bash
flutter run
```

The model dropdown at the top of the chat lists whatever you've pulled locally via `ollama pull`.

## 🎨 Design Notes

- Soft off-white background, indigo/coral accent palette, rounded chat bubbles, Inter font via `google_fonts`
- Assistant replies render as Markdown so code blocks and formatting display correctly
- Streaming is consumed line-by-line as newline-delimited JSON from Ollama's `/api/chat` endpoint — only the extracted text token is ever passed to the UI, with no request/response internals, timestamps, or debug output shown to the user

## 🗺️ Roadmap

- [ ] Persistent chat history (local storage)
- [ ] Multi-conversation / session management
- [ ] Dark theme
- [ ] Image input for multimodal models
- [ ] Voice input/output

## 🤝 Contributing

Issues and pull requests are welcome. For major changes, please open an issue first to discuss what you'd like to change.

## 📄 License

This project is licensed under the [MIT License](LICENSE).

## 🙋 Author

**Muhammad Ramish Raza**
[LinkedIn](https://www.linkedin.com/in/muhammad-ramish-raza-ab8608331) · [GitHub](https://github.com/Ramishraza2003) · [Portfolio](https://muhammadramishraza.site.je)

---

<div align="center">
<sub>Built for people who want AI in their pocket — without giving up their data.</sub>
</div>
