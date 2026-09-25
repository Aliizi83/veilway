<div align="center">

# Veilway

**Open-source, core-agnostic proxy client for Android and Linux.**

Connect to your V2Ray / Xray / sing-box configs with one tap.

![Status](https://img.shields.io/badge/status-early%20development-orange)
![Platforms](https://img.shields.io/badge/platforms-Android%20%7C%20Linux-blue)
![License](https://img.shields.io/badge/license-GPL--3.0-green)

</div>

---

> ⚠️ **Veilway is in early development.** Things will change and break. It is not ready for daily use yet.

## Features

### Planned for the first release
- Import configs from share links (`vless://`, `vmess://`, `trojan://`, `ss://`) or QR code
- Manage a list of configs and pick one to use
- One-tap connect / disconnect
- Latency (ping) test
- Android: full-device VPN via `VpnService`
- Linux: local SOCKS / HTTP proxy mode (no root required)

### Roadmap
- [ ] Subscription links with auto-update
- [ ] Auto-reconnect and network-change handling
- [ ] Routing rules (e.g. direct connection for local sites) and DNS settings
- [ ] Per-app proxy on Android
- [ ] TUN mode on Linux
- [ ] Automatic best-server selection
- [ ] Xray core support
- [ ] Windows support

## Architecture

Veilway is built so the app never depends directly on a specific proxy core.

```
┌──────────────────────────────────────────┐
│              Flutter UI (Dart)           │
├──────────────────────────────────────────┤
│  Link parser  →  Veilway config model    │
├──────────────────────────────────────────┤
│         ProxyCore interface              │
├──────────────┬──────────────┬────────────┤
│ SingBoxCore  │  XrayCore    │  (future)  │
│  adapter     │  adapter     │  adapters  │
└──────────────┴──────────────┴────────────┘
        │                │
   platform layer: Android VpnService (Kotlin) / Linux process
```

- **Config model:** share links are parsed into Veilway's own data model, not straight into a core's JSON.
- **Core adapters:** each core has an adapter that translates the model into that core's config and controls its lifecycle. Adding a new core means writing a new adapter.
- **Capabilities:** each core declares which protocols and features it supports, so the app knows which core can run which config.

## Tech stack

| Layer | Technology |
|---|---|
| UI & app logic | Flutter / Dart |
| Default core | [sing-box](https://github.com/SagerNet/sing-box) |
| Android VPN | Kotlin (`VpnService`) |
| Linux | Core runs as a managed child process |

## Building from source

### Requirements
- [Flutter SDK](https://docs.flutter.dev/get-started/install) (stable channel)
- Android Studio (for the Android SDK) and JDK 17
- Linux build tools: `clang cmake ninja-build pkg-config libgtk-3-dev`

### Steps
```bash
git clone https://github.com/Aliizi83/veilway.git
cd veilway
flutter pub get

# Android
flutter run -d android

# Linux
flutter run -d linux
```

> Detailed build instructions, including how the core binary is bundled, will be added as the project matures.

## Contributing

Issues and pull requests are welcome. Since this is an early-stage project, please open an issue to discuss larger changes before starting work on them.

## Disclaimer

Veilway is a client only. It does not provide any servers or configs. You are responsible for the servers you connect to and for complying with the laws that apply to you.

## License

Veilway is licensed under the [GNU General Public License v3.0](LICENSE).

Veilway is an independent project and is not affiliated with or endorsed by sing-box, Xray, V2Ray or Hiddify.
