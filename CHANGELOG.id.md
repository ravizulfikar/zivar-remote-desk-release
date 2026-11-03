# Catatan perubahan

[English](CHANGELOG.md)

Semua perubahan penting pada ZIVAR Remote Desk dicatat di berkas ini.

## [0.12.0] - 2026-11-04

### Diubah

- **Kunci akses kembali tertanam di aplikasi**, jadi pemasangan baru langsung tersambung tanpa mengetik apa pun. Kode akses yang diketik dan kartu Server dari 0.11.0 dihapus. Build mengambil kunci dari GitHub secret `APP_KEY`. Kunci itu ada di dalam berkas unduhan dan bisa diambil darinya; lihat README untuk akibatnya dan cara menggantinya.
- Saat server menolak kunci bawaan (kunci diganti setelah build dibuat), aplikasi memberi tahu dan meminta pengguna memasang versi terbaru, bukan menampilkan koneksi yang mati.
- **Ikon peluncur Android** kini sama dengan logo Windows. APK sebelumnya memakai ikon template bawaan karena proyek yang dihasilkan mengabaikan ikon di repositori; build sekarang menyalinnya. Ikon digambar ulang sebagai ikon adaptif (latar gradien, logo di dalam area aman) dengan versi lama dan bulat (`scripts/make-android-icons.mjs`).

## [0.11.0] - 2026-11-03

### Ditambahkan

- **Kode akses server** (Pengaturan, Server di Windows; Tentang, Server di Android): kode yang diminta Worker signaling (`APP_KEY`) diketik sekali per perangkat dan disimpan terenkripsi (DPAPI Windows). Build rilis tidak lagi memuatnya, sehingga tidak ada yang bisa diekstrak dari berkas unduhan. Alamat server juga dapat diubah di sana, dengan tombol untuk menguji sambungan. Saat kode belum diisi atau salah, halaman Beranda dan Hubungkan serta pesan koneksi menyebutkannya, bukan sekadar koneksi yang mati.

### Diubah

- **Unduhan publik**: workflow rilis menerbitkan installer dan APK ke repo publik `zivar-remote-desk-release` (bila secret `RELEASES_TOKEN` ada) sementara repo sumber tetap private. Mengunduh tidak perlu login GitHub.
- **Pembaruan** dibaca langsung dari rilis terbaru repo publik itu (dengan catatan rilis dari changelog); manifest di Worker tidak diperlukan lagi untuk aplikasi baru.

## [0.10.1] - 2026-11-03

### Ditambahkan

- **Viewer web pribadi** (`web/`, Worker `zivar-web`): sambung ke komputer Anda dari browser, dilindungi login email Cloudflare Access. Worker memverifikasi token Access sendiri dan menjangkau server signaling lewat service binding dengan rahasia bersama baru (`WEB_KEY`), sehingga app key tidak pernah dikirim ke browser. Mode layar dan terminal berfungsi; di browser tidak ada penyimpanan kata sandi dan tidak ada sambungan lewat IP lokal. Lihat README.
- Server signaling menerima `WEB_KEY` sebagai kredensial kedua di samping `APP_KEY`.
- Tombol **Keluar** di viewer web, yang mengakhiri sesi login Cloudflare Access. Tombol ini tidak muncul di aplikasi Windows dan Android.

### Catatan

- Aplikasi Windows dan Android berperilaku seperti 0.10.0; rilis ini membawa kode antarmuka bersama milik viewer web.

## [0.10.0] - 2026-11-03

### Ditambahkan

- **Riwayat akses** di halaman beranda: semua yang pernah terhubung ke komputer ini, dengan waktu, alamat IP, jalur (Internet atau LAN), layar atau terminal, hasilnya (tersambung, kata sandi salah, ditolak, terminal tidak diizinkan), dan lama sesinya. Hanya disimpan di komputer ini, maksimal 100 entri, dengan tombol untuk menghapusnya. Server signaling kini meneruskan alamat IP penyambung ke host sehingga sambungan lewat internet juga menampilkannya.

### Diubah

- Tab **Komputer Ini** menjadi **Beranda**. Isinya hanya ID komputer ini dan riwayat akses, dengan pengingat untuk menyalakan akses jarak jauh bila sedang mati.
- Semua pengaturan dipindahkan ke tab **Pengaturan** yang baru: akses jarak jauh, keamanan, jaringan lokal, layar kunci, video, latar belakang, dan proxy.
- Di ponsel tab Beranda dan Pengaturan tetap tersembunyi seperti sebelumnya (ponsel hanya sebagai viewer).

## [0.9.0] - 2026-11-03

### Ditambahkan

- **Komputer terkunci dan UAC** (layanan Windows opsional, Komputer Ini, Layar kunci): pasang sekali dengan persetujuan administrator. Helper yang berjalan sebagai SYSTEM di sesi yang sedang login mengikuti desktop yang menerima input. Saat layar kunci atau jendela UAC aktif di tengah sesi remote, video beralih ke gambar JPEG dari helper dan input mouse/keyboard dikirim ke helper, sehingga kata sandi Windows dapat diketik dari jauh. Gambar normal kembali setelah komputer dibuka.
- Helper menggambar pointer mouse di dalam gambarnya.
- Layanan berjalan dari salinan di `Program Files`; kartu menampilkan apakah layanan terpasang, berjalan, dan sama versinya dengan aplikasi, dengan tombol Pasang, Perbarui, dan Hapus.

### Diubah

- Jika penangkapan layar gagal dimulai, penangkapan dicoba ulang beberapa kali, dan bila layanan terpasang, sesi dimulai dengan gambar dari helper alih-alih gagal. Penangkapan dicoba lagi tiap beberapa detik dan gambar normal kembali saat berfungsi.
- Penangkapan layar yang berhenti sendiri (seperti saat komputer dikunci) tidak lagi mengakhiri sesi; penangkapan dicoba ulang hingga satu menit.

### Catatan

- Tidak tercakup: layar login setelah restart, ketika belum ada yang login dan aplikasinya belum berjalan.
- Gambar desktop yang dilindungi berupa JPEG sekitar 10 frame per detik, dan bisa hitam selama satu detik sesaat setelah dikunci.
- Deteksi layar kunci, penangkapannya, serta pasang/perbarui/hapus layanan sudah diuji di komputer sungguhan. Mengetik di kolom kata sandi Windows pada layar kunci yang bertahan belum diuji: di komputer uji, kunci terbuka sendiri dalam dua detik. Mohon dicoba dan laporkan hasilnya.
- Setelah komputer dikunci, penangkapan layar di mesin web aplikasi sempat tidak tersedia beberapa saat di komputer uji (browser bersih tidak terpengaruh). Ini juga berlaku tanpa layanan.

## [0.8.0] - 2026-11-03

### Ditambahkan

- **Jaringan lokal (LAN)**: hubungkan lewat alamat IP tanpa internet. Relay kecil di dalam aplikasi (port 21118) memakai pesan yang sama dengan server internet; pemeriksaan kata sandi, enkripsi, dan video tidak berubah. Viewer mengetahui ID komputer, menyimpan alamat lokal di riwayat dan mencobanya lebih dulu pada sambungan berikutnya, lalu beralih ke ID lewat internet bila gagal. Kegagalan kata sandi dibatasi seperti di server.
- **Terminal jarak jauh**: buka hanya shell (PowerShell, atau cmd) di komputer tujuan tanpa berbagi layar. Berfungsi lewat internet dan jaringan lokal, di Windows dan Android (dengan bilah tombol Esc, Tab, Ctrl, dan panah). Mati secara default (Keamanan, *Izinkan akses terminal*); kata sandi selalu diperlukan dan tantangan bertanda tangan mencakup mode yang dipilih.
- **Codec video dan akselerasi hardware** (Komputer Ini, Video): codec Otomatis / H.264 / VP9 / VP8 / AV1 dan akselerasi hardware Otomatis / Selalu gunakan GPU / Nonaktif. Mode Otomatis kini memilih H.264, yang memakai encoder kartu grafis bila ada. Viewer menampilkan codec yang dipakai, misalnya `H.264, hardware`.
- **Zoom di Android**: cubit untuk memperbesar gambar, ditambah tombol zoom dan mode *Geser* di menu mengambang. Tampilan mengikuti kursor saat diperbesar. Hanya mengubah gambar di ponsel.

### Diubah

- Seluruh teks antarmuka ditulis ulang dalam bahasa Indonesia baku yang lugas ("kata sandi" menggantikan "password", kalimat pendek yang lebih jelas).
- Kolom ID menerima ID atau alamat IP / nama komputer, dan riwayat menampilkan entri Terminal dan LAN.

### Catatan

- Viewer lama (0.7.0 ke bawah) tetap bisa tersambung ke versi ini, dan viewer ini tetap bisa tersambung ke host lama pada mode layar.
- Komputer yang terkunci atau berada di layar login masih belum bisa dikontrol.

## [0.7.0] - 2026-11-02

### Ditambahkan

- **Pengaturan proxy HTTP** (Windows): Komputer ini, Jaringan (proxy). Isi alamat dan port proxy (misalnya `10.15.3.20` dan `80`), tekan Simpan dan mulai ulang. Semua aktivitas online aplikasi memakainya: koneksi signaling, kredensial TURN, dan pemeriksaan pembaruan. Alamat yang tersimpan diperiksa dulu sebelum dipakai sehingga tidak bisa menyisipkan switch browser tambahan.
- `LICENSE` (MIT), README dalam bahasa Inggris dan Indonesia, catatan perubahan ini, dan `THIRD_PARTY_NOTICES.md`.

### Catatan

- Hanya proxy HTTP biasa tanpa login yang didukung.
- Di balik proxy, video harus lewat relay TURN melalui TCP/TLS (port 443). Belum diuji dengan proxy kantor sungguhan.

## [0.6.0] - 2026-11-02

### Ditambahkan

- **Berjalan di latar belakang**: menutup atau meminimalkan jendela menyembunyikannya ke ikon system tray selama akses jarak jauh menyala, jadi ID permanen tetap bisa dihubungi. Ikon tray menampilkan ID dan status serta menu Buka dan Keluar. Bisa dimatikan.
- **Jalankan saat Windows dimulai** (opsional): aplikasi mulai tersembunyi di tray setelah Anda login.
- Menjalankan aplikasi lagi tidak membuat host kedua; jendela yang sudah ada yang dimunculkan.
- Dengan persetujuan manual, jendela muncul saat ada yang meminta izin, tidak menunggu tak terlihat di tray.

### Diubah

- Pembatasan kinerja latar belakang pada web view dimatikan agar jendela tersembunyi tetap streaming dengan kecepatan penuh.

## [0.5.0] - 2026-11-02

### Ditambahkan

- **Periksa pembaruan** di tab Tentang yang baru (Windows dan Android), ditambah pemeriksaan diam-diam saat dibuka (paling sering tiap 12 jam) dengan titik di tab bila ada versi baru.
- **Tentang**: logo, nama, versi, dan https://zivar.my.id.
- Bar tab di bawah pada ponsel, jadi tombol tidak lagi terhalang status bar.
- Worker melaporkan status TURN di `/health` dan menyajikan versi terbaru di `/update`.

### Diubah

- Jendela desktop yang sempit menampilkan tab berupa ikon saja, dan jendela punya ukuran minimum.

## [0.4.0] - 2026-11-02

### Ditambahkan

- Logo baru dan antarmuka yang dirombak dengan tema terang dan gelap serta splash beranimasi.
- **ID permanen** yang diturunkan dari identitas komputer, jadi tetap sama walau dipasang ulang.
- **Riwayat koneksi** dengan nama dan favorit, serta **password tersimpan** (dienkripsi DPAPI Windows) untuk sambung satu ketukan.
- **Relay TURN** lewat Cloudflare Realtime.

## [0.3.0] - 2026-11-02

### Ditambahkan

- **Kualitas adaptif**: host menurunkan gambar saat jaringan lemot dan menaikkannya lagi saat membaik; viewer bisa mengunci Tinggi, Sedang, atau Rendah.
- **Penanda "sedang dikontrol"** di host: bingkai merah dan lencana selalu-di-atas dengan tombol Putuskan, keduanya tidak ikut tertangkap ke video.
- Tombol kontrol mengambang yang bisa digeser di Android.

### Diubah

- Alamat server dan app key tidak lagi ditampilkan atau bisa diubah di aplikasi.

## [0.2.1] - 2026-11-01

### Diubah

- Alamat server dan app key ditanam ke build oleh GitHub Actions, jadi tidak ada yang perlu diketik.

## [0.2.0] - 2026-11-01

### Ditambahkan

- **Viewer Android** dengan kontrol sentuh dan keyboard layar.
- GitHub Actions membangun installer Windows dan APK Android lalu menerbitkan rilis.

## [0.1.0] - 2026-11-01

### Ditambahkan

- Rilis pertama: host dan viewer untuk Windows, sambungan dengan ID 9 digit dan password, password tidak pernah dikirim (HMAC), video dan input P2P lewat WebRTC, serta server signaling Cloudflare Worker.
