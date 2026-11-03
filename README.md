# Zivar Remote Desk: unduhan

Remote desktop dengan ID 9 digit dan kata sandi: video, mouse, dan keyboard mengalir langsung antar komputer (P2P) lewat WebRTC. Host untuk Windows, viewer untuk Windows dan Android.

Repo ini hanya berisi **berkas rilis** (installer Windows dan APK Android). Kode sumbernya tidak dipublikasikan.

## Unduh

Ambil versi terbaru di halaman [Releases](../../releases/latest):

- **Windows**: `ZivarRemoteDesk_<versi>_x64-setup.exe`. Installer belum ditandatangani, jadi SmartScreen dapat memberi peringatan saat pertama dijalankan (Informasi lebih lanjut, lalu Tetap jalankan).
- **Android**: `ZivarRemoteDesk_<versi>_android.apk`. Izinkan pemasangan dari sumber tidak dikenal. Ini build debug untuk uji coba.

## Kode akses server

Aplikasi memerlukan **kode akses** dari pemilik server sebelum dapat tersambung. Kode itu sengaja tidak ada di dalam berkas unduhan. Ketik sekali di perangkat Anda:

- Windows: **Pengaturan**, bagian **Server**.
- Android: **Tentang**, bagian **Server**.

Tanpa kode, aplikasi akan menampilkan pengingat dan tidak dapat tersambung.

## Pembaruan

Menu **Tentang** di aplikasi memeriksa rilis terbaru di halaman ini dan membuka berkas unduhannya langsung.

---

*English:* this repository only hosts the release files (Windows installer and Android APK) of Zivar Remote Desk. The source code is private. The app needs a server access code from the server owner before it can connect (Settings, Server).
