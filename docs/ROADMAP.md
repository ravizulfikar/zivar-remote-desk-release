# Roadmap fitur

Dibandingkan dengan AnyDesk, RustDesk, TeamViewer, dan TryDesk. Daftar ini disusun dari fitur umum
produk-produk tersebut dan dinilai berdasarkan manfaatnya untuk kasus utama: **mengakses komputer
kantor tanpa ditunggui**. Sebagian besar hasil riset web hanya berupa ringkasan sekunder, jadi
cek fitur spesifik ke dokumentasi resmi masing-masing sebelum mengandalkannya.

Status: sudah ada = ✅, belum = ⬜.

## Sudah ada

| Fitur | Keterangan |
|---|---|
| ✅ Sambung dengan ID + password | Password tidak pernah dikirim ke server (HMAC) |
| ✅ ID permanen | Diturunkan dari identitas komputer, tetap sama walau dipasang ulang |
| ✅ Riwayat + simpan password | Sambung satu ketukan, favorit, nama, password dienkripsi DPAPI (Windows) |
| ✅ Kualitas adaptif | Turun otomatis saat lemot, bisa dikunci Tinggi/Sedang/Rendah |
| ✅ Penanda "sedang dikontrol" | Bingkai merah + lencana Putuskan di komputer host |
| ✅ Persetujuan manual | Opsional, minta izin tiap ada yang menyambung |
| ✅ Kontrol sentuh Android | Touchpad, klik, scroll, drag, keyboard layar |
| ✅ TURN (relay) aktif | Cadangan saat sambungan langsung diblokir; cek di `/health` |
| ✅ Periksa pembaruan | Menu Tentang (Windows dan Android) + cek otomatis saat buka. Unduhan lewat browser karena repo private |
| ✅ Berjalan di latar belakang | System tray: tutup/minimize menyembunyikan jendela, ID tetap online. Opsi jalan otomatis setelah login Windows. Satu instance saja |
| ✅ Proxy HTTP | Pengaturan alamat dan port proxy (mis. 10.15.3.20:80) di kartu Jaringan; berlaku untuk signaling, TURN, dan cek pembaruan. Tanpa login proxy; belum diuji dengan proxy kantor sungguhan |
| ✅ Beranda, Pengaturan, dan riwayat akses | Beranda berisi ID dan riwayat siapa yang mengakses (waktu, IP, jalur, mode, hasil, durasi); semua pengaturan ada di tab Pengaturan |
| ✅ Komputer terkunci dan UAC | Layanan Windows opsional + helper SYSTEM; layar kunci dan UAC terlihat dan bisa dikontrol saat sesi masih login. Belum tercakup: layar login setelah restart |
| ✅ Mode LAN | Sambung lewat alamat IP tanpa internet (relay di dalam aplikasi, port 21118), dengan fallback ke ID |
| ✅ Terminal jarak jauh | Hanya shell, tanpa layar; mati secara default; Windows dan Android |
| ✅ Codec video dan hardware | Otomatis / H.264 / VP9 / VP8 / AV1, akselerasi GPU otomatis / paksa / mati; codec yang dipakai ditampilkan |
| ✅ Zoom di Android | Cubit, tombol zoom, mode geser; hanya tampilan di ponsel |
| ✅ Tentang | Logo, nama, versi, dan tautan zivar.my.id |

## Antrean berikutnya (disimpan)

Urutan yang dipilih untuk dikerjakan nanti:

0. Layar login setelah restart (belum ada yang login): host harus berjalan sebagai layanan, bukan sebagai aplikasi pengguna. Pondasi layanan dan helper sudah ada sejak 0.9.0
1. Windows Service dan jalan otomatis saat boot (layar login dan UAC bisa dikontrol). Tray + auto-start setelah login sudah ada; sisanya butuh Service
2. Transfer file dua panel
3. Sinkron clipboard
4. Multi-monitor
5. Mode privasi (layar komputer kantor digelapkan, input lokal dikunci selama sesi)
6. Autentikasi dua langkah (TOTP) dan izin per sesi (mis. hanya lihat)
7. Auto-update yang memasang sendiri, dan penandatanganan kode (hilangkan peringatan SmartScreen,
   APK bisa menimpa versi lama)

(TURN sudah aktif; "Periksa pembaruan" sudah ada, tapi masih mengarahkan ke unduhan manual.)

## Prioritas tinggi (paling berguna untuk akses kantor)

| # | Fitur | Mengapa | Catatan teknis |
|---|---|---|---|
| 1 | ⬜ **Windows Service + jalan otomatis saat boot** | Komputer kantor bisa diakses setelah restart tanpa ada yang membuka aplikasi. Juga membuka kontrol layar login dan dialog UAC | Layanan SYSTEM + proses di sesi pengguna; ini fitur terbesar |
| 2 | ✅ **TURN aktif** | Tanpa relay, sebagian jaringan kantor/seluler gagal tersambung | Sudah aktif; uji dari jaringan kantor/seluler |
| 3 | ⬜ **Transfer file** (dua panel, seret-lepas) | Salah satu alasan utama orang memakai AnyDesk/TeamViewer/TryDesk | Data channel WebRTC terpisah, dipecah per potongan |
| 4 | ⬜ **Sinkron clipboard** (teks dulu, lalu file/gambar) | Menyalin-menempel antar komputer | Data channel + izin per sesi |
| 5 | ⬜ **Multi-monitor** (pilih/ganti layar) | Komputer kantor sering punya 2 layar | Pilih sumber capture, kirim daftar monitor |
| 6 | ⬜ **Mode privasi / blokir input lokal** | Layar host digelapkan dan input lokal dikunci selama sesi | Ada di RustDesk, AnyDesk, TryDesk |
| 7 | ⬜ **Tombol khusus** (Ctrl+Alt+Del, Win, Alt+Tab) | Kombinasi yang tidak bisa dikirim dari keyboard biasa | Bergantung fitur 1 untuk Ctrl+Alt+Del |
| 8 | ⬜ **Auto-update yang memasang sendiri** | Saat ini hanya memberi tahu dan membuka halaman unduhan | Tauri updater + rilis bertanda tangan + sumber unduhan yang tidak butuh login |

## Prioritas menengah

| # | Fitur | Catatan |
|---|---|---|
| 9 | ⬜ Autentikasi dua langkah (TOTP) di host | Tersedia di RustDesk; menutup risiko password bocor |
| 10 | ⬜ Izin per sesi (hanya lihat, tanpa clipboard/file) | Mode "lihat saja" ada di TryDesk, AnyDesk, TeamViewer |
| 11 | ✅ Log koneksi di host (siapa, kapan, berapa lama) | Selesai di 0.10.0 (riwayat akses di Beranda) |
| 12 | ⬜ Audio dari komputer jauh | Capture audio sistem lewat `getDisplayMedia` |
| 13 | ⬜ Chat dua arah | Pesan singkat selama sesi |
| 14 | ⬜ Rekam sesi (.webm) dan screenshot | `MediaRecorder` di sisi viewer |
| 15 | ✅ Mode LAN langsung (IP tanpa server) | Selesai di 0.8.0 |
| 16 | ⬜ Wake-on-LAN | Bangunkan komputer tidur lewat perangkat lain di LAN yang sama (AnyDesk punya) |
| 17 | ⬜ FPS tinggi, mode game | Pilihan codec dan hardware sudah ada (0.8.0); sisanya belum |
| 18 | ⬜ Pilih bahasa (ID/EN) dan tema manual | Saat ini mengikuti sistem |

## Prioritas rendah / besar

| # | Fitur | Catatan |
|---|---|---|
| 19 | ⬜ Address book yang tersinkron antar perangkat (akun) | Perlu server akun (mis. Cloudflare D1) |
| 20 | ⬜ Host untuk Android | MediaProjection + AccessibilityService; kebijakan Play Store ketat |
| 21 | ⬜ Host macOS/Linux | macOS: izin Screen Recording + Accessibility. Linux Wayland: PipeWire + portal |
| 22 | ⬜ Viewer iOS | Perlu Mac untuk build. iOS tidak bisa jadi host |
| 23 | ⬜ Tunnel TCP / printer jarak jauh | Ada di RustDesk Pro / TeamViewer |
| 24 | ⬜ Penandatanganan kode (Windows, Android rilis) | Menghilangkan peringatan SmartScreen dan memungkinkan update APK di atas versi lama |

## Keamanan yang masih perlu dikerjakan

- Dukungan proxy dengan username/password, dan uji di balik proxy kantor sungguhan (relay TURN lewat TCP/TLS 443).
- Penyimpanan password di Android memakai Android Keystore (saat ini hanya dilindungi sandbox aplikasi).
- Batasi percobaan password per ID secara permanen (saat ini hanya di memori Durable Object).
- Daftar izin perangkat (hanya ID tertentu yang boleh menyambung).
