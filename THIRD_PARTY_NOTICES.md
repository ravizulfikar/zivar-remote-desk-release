# Third-party notices

ZIVAR Remote Desk's own code is released under the [MIT License](LICENSE). The app is built on the
components below, each under its own license. Only the direct dependencies of the application are
listed; their own dependencies carry their own licenses, which you can list with `cargo metadata`
(Rust) or `npm ls` (JavaScript).

## Desktop and Android app (Rust)

| Component | Use | License |
|---|---|---|
| [Tauri](https://tauri.app) 2 | App framework, system tray, windows | Apache-2.0 OR MIT |
| tauri-plugin-opener | Opens links in the system browser | Apache-2.0 OR MIT |
| tauri-plugin-single-instance | Allows only one running copy (desktop) | Apache-2.0 OR MIT |
| [enigo](https://github.com/enigo-rs/enigo) | Mouse and keyboard input on the host (desktop) | MIT |
| [windows](https://github.com/microsoft/windows-rs) | Windows DPAPI for saved passwords | MIT OR Apache-2.0 |
| [winreg](https://github.com/gentoo90/winreg-rs) | Reads the machine identity, start-with-Windows entry | MIT |
| [serde](https://serde.rs) and serde_json | Reading and writing the settings file | MIT OR Apache-2.0 |
| [portable-pty](https://github.com/wez/wezterm/tree/main/pty) | Pseudo-terminal (ConPTY) for the remote terminal | MIT |
| [tokio](https://tokio.rs) | Async runtime for the local-network relay | MIT |
| [tokio-tungstenite](https://github.com/snapview/tokio-tungstenite) | WebSocket server of the local-network relay | MIT |
| [futures-util](https://github.com/rust-lang/futures-rs) | Stream and sink helpers | MIT OR Apache-2.0 |
| [windows-service](https://github.com/mullvad/windows-service-rs) | Windows service for the lock-screen helper | MIT OR Apache-2.0 |
| [jpeg-encoder](https://github.com/vstroebel/jpeg-encoder) | JPEG pictures of the lock screen | (MIT OR Apache-2.0) AND IJG |

jpeg-encoder includes material under the IJG license. This software is based in part on the work of the Independent JPEG Group.

## Bundled web components

| Component | Use | License |
|---|---|---|
| [xterm.js](https://xtermjs.org) 5.5.0 and its fit addon 0.10.0 | Terminal display in the viewer (`client/src/vendor/xterm`, license file included) | MIT |

## Runtime provided by the system

- **Microsoft Edge WebView2** (Windows) renders the interface and carries the WebRTC connection. It is
  a Microsoft component under Microsoft's license terms and is not part of this repository.
- **Android System WebView** (Android) plays the same role on Android.

## Server

| Component | Use | License |
|---|---|---|
| [Cloudflare Workers](https://workers.cloudflare.com), Durable Objects, KV | Signaling server | Cloudflare service terms |
| [Cloudflare Realtime TURN](https://developers.cloudflare.com/realtime/turn/) | Relay for connections that cannot go direct | Cloudflare service terms |
| [Wrangler](https://developers.cloudflare.com/workers/wrangler/) | Deploy tool (development only) | MIT OR Apache-2.0 |

## Build

GitHub Actions builds the installers with the Rust toolchain, Node.js, the Tauri CLI and the Android SDK
and NDK. None of these tools is included in the app's source.
