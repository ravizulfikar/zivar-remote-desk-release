# Changelog

[Bahasa Indonesia](CHANGELOG.id.md)

All notable changes to ZIVAR Remote Desk are recorded in this file.

## [0.12.0] - 2026-11-04

### Changed

- **The access key is built into the apps again**, so a fresh install connects at once with nothing to type. The typed access code and the Server card of 0.11.0 are gone. Builds get the key from the GitHub secret `APP_KEY`. The key is inside the downloadable files and can be extracted from them; see the README for what that allows and how to replace the key.
- When the server turns the built-in key down (the key was replaced after the build), the app says so and tells the user to install the newest version, instead of showing a dead connection.
- **Android launcher icon** now matches the Windows logo. The APK used the default template icon because the generated project ignored the icons in the repository; the build now copies them in. The icon is redrawn as an adaptive icon (gradient background, logo inside the safe area) with legacy and round versions (`scripts/make-android-icons.mjs`).

## [0.11.0] - 2026-11-03

### Added

- **Server access code** (Settings, Server on Windows; About, Server on Android): the code that the signaling Worker requires (`APP_KEY`) is typed once per device and kept encrypted (Windows DPAPI). Released builds no longer contain it, so nothing can be extracted from the downloads. The server address can be changed there too, with a button that tests the connection. When the code is missing or wrong, the Home and Connect pages and the connection messages say so instead of showing a dead connection.

### Changed

- **Public downloads**: the release workflow publishes the installer and APK to the public repository `zivar-remote-desk-release` (when the `RELEASES_TOKEN` secret exists) while the source repository stays private. Downloading needs no GitHub login.
- **Updates** are read straight from the latest release of that public repository (with the release notes from the changelog); the Worker manifest is no longer needed for new apps.

## [0.10.1] - 2026-11-03

### Added

- **Private web viewer** (`web/`, Worker `zivar-web`): connect to your computers from a browser, protected by Cloudflare Access e-mail sign-in. The Worker verifies the Access token itself and reaches the signaling server through a service binding with a new shared secret (`WEB_KEY`), so the app key is never sent to a browser. Screen and terminal modes work; no stored passwords and no local-IP connections in the browser. See the README.
- The signaling server accepts `WEB_KEY` as a second credential next to `APP_KEY`.
- A **Keluar** (sign out) button in the web viewer, which ends the Cloudflare Access session. It does not appear in the Windows and Android apps.

### Notes

- The Windows and Android apps behave as in 0.10.0; this release carries the shared interface code of the web viewer.

## [0.10.0] - 2026-11-03

### Added

- **Access history** on the home page: everyone who connected to this computer, with time, IP address, route (Internet or LAN), screen or terminal, the result (connected, wrong password, refused, terminal not allowed) and the length of the session. Stored only on this computer, up to 100 entries, with a button to clear it. The signaling server now passes the viewer's IP address to the host so internet connections show it too.

### Changed

- The **This computer** tab is now **Home**. It shows only this computer's ID and the access history, with a reminder to switch on remote access when it is off.
- All settings moved to a new **Settings** tab: remote access, security, local network, lock screen, video, background and proxy.
- On phones the Home and Settings tabs stay hidden, as before (the phone is a viewer only).

## [0.9.0] - 2026-11-03

### Added

- **Locked computer and UAC** (optional Windows service, This computer, Lock screen): install it once with an administrator prompt. A helper running as SYSTEM in the signed-in session follows the desktop that receives input. While the lock screen or a UAC prompt is active during a remote session, the video switches to the helper's JPEG pictures and mouse/keyboard input goes to the helper, so the Windows password can be typed remotely. The normal picture returns after unlock.
- The helper draws the mouse pointer into its pictures.
- The service runs from a copy in `Program Files`; the card shows whether it is installed, running and the same version as the app, with Install, Update and Remove buttons.

### Changed

- If the screen capture fails to start, it is retried a few times, and when the service is installed the session starts on the helper's pictures instead of failing. The capture is tried again every few seconds and the normal picture returns when it works.
- A screen capture that ends by itself (as it does when the computer is locked) no longer ends the session; it is retried for up to a minute.

### Notes

- Not covered: the sign-in screen after a restart, when nobody has signed in yet and the app is not running.
- The pictures of the protected desktop are JPEG at about 10 frames per second, and can be black for a second right after locking.
- Detecting the lock screen, capturing it, and the service install/update/remove were tested on a real computer. Typing into the Windows password box on a lock screen that stays up was not: on the test computer the lock opened itself within two seconds. Please try it and report what you see.
- After the computer was locked, the screen capture inside the app's web engine stayed unavailable for a while on the test computer (a clean browser was not affected). This also applies without the service.

## [0.8.0] - 2026-11-03

### Added

- **Local network (LAN)**: connect by IP address without the internet. A small relay inside the app (port 21118) speaks the same messages as the internet server; the password check, encryption and video are unchanged. The viewer learns the computer's ID, saves the local address in the history and tries it first next time, falling back to the ID over the internet. Password failures are limited the same way as on the server.
- **Remote terminal**: open only a shell (PowerShell, or cmd) on the other computer, with no screen sharing. Works over the internet and the local network, on Windows and Android (with a key bar for Esc, Tab, Ctrl and arrows). Off by default (Security, *Allow terminal access*); the password is always required and the signed challenge covers the chosen mode.
- **Video codec and hardware acceleration** (This computer, Video): codec Auto / H.264 / VP9 / VP8 / AV1 and hardware acceleration Auto / Always use GPU / Off. Auto now prefers H.264, which uses the graphics card's encoder when there is one. The viewer shows the codec in use, for example `H.264, hardware`.
- **Zoom on Android**: pinch to zoom the picture, plus zoom buttons and a *Move view* mode in the floating menu. The view follows the cursor when zoomed. It only changes the picture on the phone.

### Changed

- All interface text was rewritten in plain standard Indonesian ("kata sandi" instead of "password", clearer short sentences).
- The ID field accepts an ID or an IP address / computer name, and the history shows Terminal and LAN entries.

### Notes

- Older viewers (0.7.0 and before) can still connect to this version, and this viewer can still connect to older hosts in screen mode.
- A computer that is locked or at the sign-in screen still cannot be controlled.

## [0.7.0] - 2026-11-02

### Added

- **HTTP proxy setting** (Windows): This computer, Network (proxy). Enter the proxy address and port (for example `10.15.3.20` and `80`), press Save and restart. Every online action of the app uses it: the signaling connection, the TURN credentials and the update check. The saved address is checked before use so it can never add extra browser switches.
- A `LICENSE` (MIT), README in English and Indonesian, this changelog and `THIRD_PARTY_NOTICES.md`.

### Notes

- Only a plain HTTP proxy without a login is supported.
- Behind a proxy the video must go through the TURN relay over TCP/TLS (port 443). This has not been tested against a real office proxy.

## [0.6.0] - 2026-11-02

### Added

- **Runs in the background**: closing or minimizing the window hides it to a system tray icon while remote access is on, so the permanent ID stays reachable. The tray icon shows the ID and status and has Open and Exit. Can be turned off.
- **Start with Windows** (optional): the app starts hidden in the tray after you sign in.
- Starting the app again no longer starts a second host; it brings the running window forward.
- With manual approval on, the window pops up when someone asks for permission instead of waiting unseen in the tray.

### Changed

- Background throttling of the web view is disabled so a hidden window keeps streaming at full speed.

## [0.5.0] - 2026-11-02

### Added

- **Check for updates** in a new About tab (Windows and Android), plus a quiet check at start-up (at most every 12 hours) with a dot on the tab when a newer version exists.
- **About**: logo, name, version and https://zivar.my.id.
- A bottom tab bar on phones, so the buttons are no longer hidden behind the status bar.
- The Worker reports the TURN status at `/health` and serves the latest version at `/update`.

### Changed

- Narrow desktop windows show icon-only tabs, and the window has a minimum size.

## [0.4.0] - 2026-11-02

### Added

- A new logo and a redesigned interface with light and dark themes and an animated splash.
- **Permanent ID** derived from the computer's identity, so it survives reinstalling.
- **Connection history** with names and favourites, and **saved passwords** (encrypted with Windows DPAPI) for one-tap connections.
- **TURN relay** through Cloudflare Realtime.

## [0.3.0] - 2026-11-02

### Added

- **Adaptive quality**: the host steps the picture down on a slow network and back up when it recovers; the viewer can pin High, Medium or Low.
- **"Being controlled" marker** on the host: a red frame and an always-on-top badge with a Disconnect button, both hidden from the capture.
- A floating, movable control button on Android.

### Changed

- The server address and app key are no longer shown or editable in the app.

## [0.2.1] - 2026-11-01

### Changed

- The server address and app key are baked into the builds by GitHub Actions, so nothing has to be typed in.

## [0.2.0] - 2026-11-01

### Added

- **Android viewer** with touch control and an on-screen keyboard.
- GitHub Actions builds the Windows installer and the Android APK and publishes the release.

## [0.1.0] - 2026-11-01

### Added

- First release: host and viewer for Windows, connection by 9-digit ID and password, password never sent (HMAC), peer-to-peer video and input over WebRTC, and the Cloudflare Worker signaling server.
