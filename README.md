<p align="center">
  <img src="client/branding/logo.png" width="128" alt="ZIVAR Remote Desk logo">
</p>

<h1 align="center">ZIVAR Remote Desk</h1>

<p align="center">
  Control your computer from anywhere with just an ID and a password.<br>
  Free remote desktop for Windows and Android.
</p>

<p align="center">
  <a href="https://github.com/ravizulfikar/zivar-remote-desk-release/releases/latest"><img alt="release" src="https://img.shields.io/github/v/release/ravizulfikar/zivar-remote-desk-release?label=release"></a>
  <a href="LICENSE"><img alt="license" src="https://img.shields.io/github/license/ravizulfikar/zivar-remote-desk-release?label=license"></a>
</p>

<p align="center">
  <b>English</b> · <a href="README.id.md">Bahasa Indonesia</a>
</p>

<p align="center">
  <a href="https://github.com/ravizulfikar/zivar-remote-desk-release/releases/latest">Download</a> ·
  <a href="CHANGELOG.md">Changelog</a> ·
  <a href="LICENSE">MIT License</a>
</p>

---

## What is ZIVAR Remote Desk?

An app to see and control another computer from a distance, as if you were sitting in front of it. Useful for reaching your office or home computer from a laptop, another computer or an Android phone.

There is no account to create. Every computer has a **9-digit ID** and a **password** that you choose. Whoever knows both can connect.

## Features

**Connecting**
- **Permanent ID**: a computer's ID does not change, even after the app is reinstalled.
- **Your password is never sent** to anyone when you connect.
- **Connection history**: computers you have connected to are remembered with a name and a favourite flag. One tap reconnects, and the password can be remembered safely on Windows.
- **Local network**: at the office or at home, connect with the computer's IP address, even without internet.
- **Office networks with a proxy**: a proxy setting is built into the app.

**While controlling**
- **Smooth picture**: the quality adapts to your connection speed by itself. You can also pick High, Medium or Low.
- **Terminal only**: open just a command line (PowerShell) on the other computer without seeing its screen. It is off by default and must be switched on by the computer's owner.
- **Locked computer**: see and control a computer that is locked, including typing the Windows password and answering administrator permission (UAC) prompts. This needs a one-time administrator approval to install the extra component.

**On the computer being controlled**
- **A clear sign**: while it is being controlled, a red frame appears around the screen with an always-on-top **Disconnect** button.
- **Ask first**: an option to ask for permission every time someone connects.
- **Home with an access history**: see who connected, when and for how long.
- **Quietly in the background**: closing the window only hides it to an icon in the notification area, so the computer stays reachable. It can also start with Windows.

**On Android**
- **Touch control**: drag to move the cursor, tap to click, two-finger tap to right-click, two-finger drag to scroll, hold and drag to drag.
- **Zoom**: pinch to enlarge the picture on your phone (this does not change the other computer's screen), plus an on-screen keyboard and a floating control button.

**Also**
- A notice when a new version is available, in the **About** menu.
- Light and dark themes that follow your device.

## Download and install

Get the latest version from the [Releases](https://github.com/ravizulfikar/zivar-remote-desk-release/releases/latest) page:

- **Windows**: `ZivarRemoteDesk_<version>_x64-setup.exe`. The installer is not officially signed yet, so Windows SmartScreen may warn on first run: choose **More info**, then **Run anyway**.
- **Android**: `ZivarRemoteDesk_<version>_android.apk`. Allow installing from unknown sources. This is a test version (not from the Play Store).

The computer being controlled must run Windows. Android is only for controlling other computers.

## Getting started

**1. Prepare the computer you want to control (Windows)**
1. Install and open the app.
2. Open **Settings**, type a password (at least 8 characters) and save it.
3. Switch on **Allow remote access**.
4. Note the **ID** shown on **Home**.

**2. Connect from another device**
1. Install and open the app (Windows or Android).
2. Open **Connect** and type the other computer's ID and password.
3. Pick **Screen view** or **Terminal only**, then press **Connect**.

The computer you connect to must be switched on and online (or on the same network as you for a local connection).

## Tips

- **Local connection**: switch it on in **Settings**, **Local network**. The app shows the computer's IP address. Windows may ask for firewall permission; allow it on private networks.
- **Locked computer**: install the extra component in **Settings**, **Lock screen** (needs administrator approval once). After updating the app, press **Update service** in the same place.
- **Office computer behind a proxy**: enter the address and port in **Settings**, **Proxy**.
- **Stay reachable**: leave **Keep running in the notification area** on so the computer stays reachable after its window is closed. To quit completely, right-click its icon and choose **Exit**.

## Using it safely

- Use a long, unique password and do not share it with anyone you do not trust.
- Switch on **Ask for manual approval** if other people use the computer.
- Switch on **Allow terminal access** only when you need it.
- Look at the **Access history** on Home from time to time.
- A red frame around the screen means the computer is being controlled. **Disconnect** ends the session at any time.

## License

Released under the [MIT License](LICENSE). The app uses some third-party components; they are listed in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
