<p align="center">
  <img src="client/branding/logo.png" width="128" alt="Logo ZIVAR Remote Desk">
</p>

<h1 align="center">ZIVAR Remote Desk</h1>

<p align="center">
  Remote desktop dengan ID 9 digit dan kata sandi. Video, mouse, dan keyboard mengalir langsung antar komputer (P2P) lewat WebRTC;<br>
  Cloudflare Worker gratis hanya mempertemukan kedua komputer. Host untuk Windows, viewer untuk Windows dan Android.
</p>

<p align="center">
  <a href="https://github.com/ravizulfikar/zivar-remote-desk-release/releases/latest"><img alt="release" src="https://img.shields.io/github/v/release/ravizulfikar/zivar-remote-desk-release?label=release"></a>
  <a href="LICENSE"><img alt="license" src="https://img.shields.io/github/license/ravizulfikar/zivar-remote-desk-release?label=license"></a>
</p>

<p align="center">
  <a href="README.md">English</a> · <b>Bahasa Indonesia</b>
</p>

<p align="center">
  <a href="https://github.com/ravizulfikar/zivar-remote-desk-release/releases/latest">Unduh</a> ·
  <a href="CHANGELOG.id.md">Catatan perubahan</a> ·
  <a href="docs/ROADMAP.md">Roadmap</a> ·
  <a href="LICENSE">Lisensi MIT</a>
</p>

---

```
signaling/   Cloudflare Worker + Durable Object: ID 9 digit, relay SDP/ICE, batas percobaan kata sandi, kredensial TURN
client/      Tauri (Rust + WebView2): mode Host (dikontrol) dan Viewer (mengontrol)
```

## Fitur

**Menyambung**
- Sambung dengan **ID 9 digit dan kata sandi**. Kata sandi tidak pernah dikirim: host mengirim nonce, viewer membalas HMAC-SHA256 dari kunci PBKDF2 (200.000 iterasi).
- **ID permanen** (Windows): diturunkan dari identitas komputer (MachineGuid), jadi tetap sama walau aplikasi dipasang ulang. Jika kebetulan bentrok dengan komputer lain, ID bergeser satu kali lalu tetap permanen.
- **Riwayat koneksi**: setiap komputer yang pernah disambungi tersimpan dengan nama, favorit, dan waktu terakhir. Centang *Ingat kata sandi* dan sambungan berikutnya cukup satu ketukan. Kata sandi dienkripsi dengan DPAPI Windows.
- **Relay TURN** sebagai cadangan saat sambungan langsung diblokir (Cloudflare Realtime TURN).
- **Jaringan lokal (LAN)**: hubungkan lewat alamat IP, tanpa internet. Lihat [Jaringan lokal](#jaringan-lokal-lan).
- **Komputer terkunci dan UAC** (layanan Windows opsional): layar kunci dan jendela UAC tetap terlihat dan bisa dikontrol, termasuk mengetik kata sandi Windows. Lihat [Komputer terkunci](#komputer-terkunci-dan-uac).
- **Terminal saja**: buka hanya shell (PowerShell) di komputer tujuan, tanpa layar. Lihat [Terminal jarak jauh](#terminal-jarak-jauh).
- **Proxy HTTP** untuk jaringan yang hanya bisa ke internet lewat proxy (misalnya `10.15.3.20` port `80`). Lihat [Memakai proxy](#memakai-proxy).

**Selama sesi**
- **Kualitas adaptif**: host mengukur RTT, packet loss, dan bandwidth tiap 2 detik, lalu menurunkan kualitas gambar bertahap (8 Mbps 1080p30 sampai 0,5 Mbps ~360p15) saat jaringan lemot dan menaikkannya lagi saat membaik. Viewer bisa mengunci Tinggi, Sedang, atau Rendah dan melihat resolusi, fps, bitrate, dan ping.
- **Penanda "sedang dikontrol"** (Windows): bingkai merah di tepi layar dan lencana selalu-di-atas dengan tombol **Putuskan**. Keduanya tidak ikut tertangkap ke video, jadi viewer tidak melihatnya.
- **Persetujuan manual** (opsional): tanya dulu setiap ada yang menyambung.
- **Codec video dan akselerasi hardware**: mode Otomatis memilih H.264 dan memakai encoder kartu grafis bila ada. Codec yang dipakai, beserta apakah berjalan di hardware atau software, ditampilkan di viewer. Lihat [Video](#video-codec-dan-hardware).
- **Viewer Android**: kontrol seperti touchpad (geser untuk menggerakkan, ketuk untuk klik, ketuk 2 jari untuk klik kanan, geser 2 jari untuk scroll, tahan lalu geser untuk drag), keyboard layar, dan tombol kontrol mengambang yang bisa digeser. **Cubit untuk memperbesar** gambar di ponsel (hanya di ponsel, komputer tujuan tidak terpengaruh).

**Di komputer host**
- **Berjalan di latar belakang**: menutup atau meminimalkan jendela menyembunyikannya ke system tray dan ID tetap online. Bisa juga otomatis jalan saat Windows dimulai, tersembunyi di tray. Hanya satu salinan yang berjalan. Lihat [Berjalan di latar belakang](#berjalan-di-latar-belakang-windows).
- **Beranda dan riwayat akses**: tab **Beranda** menampilkan ID komputer ini dan daftar semua yang pernah terhubung ke komputer ini: waktu, alamat IP, jalur (Internet atau LAN), layar atau terminal, hasilnya (tersambung, kata sandi salah, ditolak, terminal tidak diizinkan), dan lama sesinya. Daftar ini hanya disimpan di komputer ini dan dapat dihapus. Semua pengaturan ada di tab **Pengaturan**.
- **Periksa pembaruan dan Tentang**: menampilkan logo, nama, versi, dan https://zivar.my.id, serta memberi tahu bila ada versi baru.
- Splash saat dibuka, tema terang dan gelap mengikuti sistem, dan tampilan ponsel dengan bar tab di bawah.

## Unduh dan pasang

Installer Windows dan APK Android dibangun oleh GitHub Actions untuk setiap tag `v*` di repo sumber yang private, lalu dipublikasikan di repo publik [zivar-remote-desk-release](https://github.com/ravizulfikar/zivar-remote-desk-release/releases) (mengunduh tidak perlu login GitHub).

- **Windows**: `ZivarRemoteDesk_<versi>_x64-setup.exe`. Installer belum ditandatangani, jadi SmartScreen bisa memberi peringatan saat pertama dijalankan (Informasi lebih lanjut, lalu Tetap jalankan).
- **Android**: `ZivarRemoteDesk_<versi>_android.apk`. Izinkan pemasangan dari sumber tidak dikenal. Ini build debug untuk uji coba dan tidak bisa menimpa build rilis yang ditandatangani kunci lain.

Alamat server dan kunci akses sudah tertanam di aplikasi, jadi pemasangan baru langsung tersambung: tidak ada yang perlu diatur atau diketik. Kunci itu ada di dalam berkas unduhan, sehingga siapa pun yang memegang berkasnya dapat mengambilnya dan memakai server serta jatah TURN Anda (bukan komputer Anda, yang tetap dilindungi ID dan kata sandinya). Lihat [Menjalankan server sendiri](#menjalankan-server-sendiri-cloudflare-gratis).

### Cara pakai

1. Di komputer yang akan dikontrol: tab **Pengaturan**, atur kata sandi (minimal 8 karakter) dan nyalakan **Izinkan akses jarak jauh**. ID komputer tampil di **Beranda**.
2. Di perangkat lain: tab **Hubungkan**, isi ID dan kata sandi, tekan **Sambung**.

### Kontrol di Android

Satu jari menggeser kursor, ketuk = klik kiri, ketuk 2 jari = klik kanan, dua jari menggeser = scroll, tahan lalu geser = drag. Tombol bulat mengambang (bisa digeser) membuka info koneksi, **Kualitas** (Otomatis / Tinggi / Sedang / Rendah), **Keyboard**, dan **Putus**. Android hanya viewer; belum bisa dikontrol.

## Jaringan lokal (LAN)

Di **Pengaturan**, kartu **Jaringan lokal (LAN)**: nyalakan *Izinkan sambungan langsung dari jaringan lokal*. Kartu itu lalu menampilkan alamat IP komputer ini. Dari komputer lain di jaringan yang sama, ketik alamat tersebut (misalnya `192.168.1.20`) sebagai pengganti ID di **Hubungkan**.

- Tidak perlu internet. Pemeriksaan kata sandi, enkripsi, dan video sama seperti lewat internet.
- Relay kecil di dalam aplikasi mendengarkan di port **21118**. Windows dapat menampilkan peringatan firewall saat pertama kali; izinkan pada jaringan privat.
- Viewer mengetahui ID komputer dari sambungan itu dan menyimpannya di riwayat bersama alamat lokalnya. Pada sambungan berikutnya alamat lokal dicoba lebih dulu, dan jika tidak ada jawaban, viewer beralih ke ID lewat internet.
- Kata sandi salah dibatasi seperti di server: lima kali gagal mengunci alamat itu selama 15 menit.

## Terminal jarak jauh

Nyalakan *Izinkan akses terminal* di bagian **Keamanan** pada komputer tujuan. Di **Hubungkan**, pilih **Terminal saja** di samping ID atau alamat IP. Anda mendapat terminal (PowerShell, atau cmd bila tidak ada) yang berjalan sebagai pengguna yang menjalankan aplikasi; layar tidak dibagikan dan tidak ada yang direkam.

- Berfungsi lewat internet dan jaringan lokal, juga di Android (dengan bilah tombol Esc, Tab, Ctrl, dan panah).
- Terminal sama kuasanya dengan kontrol penuh atas komputer. Fitur ini mati secara default dan kata sandi selalu diperlukan. Jika persetujuan manual aktif, pemilik ditanya lebih dulu.
- Mengetik `exit` mengakhiri sesi di kedua sisi. Shell ditutup saat viewer terputus.

## Komputer terkunci dan UAC

Program biasa tidak bisa melihat atau mengetik di layar kunci Windows maupun jendela UAC, karena Windows menampilkannya di desktop terpisah yang dilindungi. Untuk menjangkaunya, pasang layanan Windows kecil:

1. Di **Pengaturan**, kartu **Layar kunci**, tekan **Pasang layanan**.
2. Setujui jendela izin administrator Windows (UAC). Ini hanya diperlukan sekali.

Setelah itu, saat komputer terkunci atau muncul jendela UAC di tengah sesi remote, viewer tetap menampilkan layar dan mouse serta keyboard tetap berfungsi, sehingga Anda dapat mengetik kata sandi Windows dari jauh. Setelah komputer dibuka, gambar normal yang lebih mulus kembali.

Cara kerjanya: layanan (akun Windows SYSTEM) menjaga sebuah helper tetap berjalan di sesi yang sedang login. Helper mengikuti desktop yang sedang menerima input, mengirimnya sebagai gambar JPEG, dan menyuntikkan input mouse dan keyboard di sana. Aplikasi mengalihkan video ke gambar tersebut selama desktop yang dilindungi aktif.

Perlu diketahui:

- Gambar desktop yang dilindungi berupa JPEG sekitar 10 frame per detik, jadi kurang mulus dibanding gambar normal. Sesaat setelah dikunci, gambar bisa hitam selama sekitar satu detik.
- Cakupannya komputer yang dikunci **saat Anda masih login** (aplikasi tetap berjalan). Layar login setelah restart, ketika belum ada yang login, tidak tercakup.
- Setelah komputer dikunci, penangkapan layar Windows di mesin web aplikasi dapat tidak tersedia untuk sementara. Sesi lalu berlanjut memakai gambar dari layanan dan kembali ke gambar normal ketika penangkapan pulih. Tanpa layanan, sesi baru bisa gagal dimulai pada masa itu; aplikasi mencoba ulang beberapa kali.
- Layanan berjalan dari salinannya sendiri di `Program Files` (tempat yang tidak dapat ditulisi pengguna biasa). Setelah memperbarui aplikasi, tekan **Perbarui layanan** di kartu yang sama agar versinya sama. **Hapus layanan** mencopot layanan; lakukan ini sebelum menghapus aplikasi.
- Siapa pun yang mengetahui ID dan kata sandi akses dapat mengetik kata sandi Windows dari jauh. Jaga kerahasiaan kata sandi akses. Program milik pengguna Windows yang sedang login juga dapat berhubungan dengan helper, seperti halnya mereka sudah dapat mengontrol desktop normal.

## Video: codec dan hardware

Di **Pengaturan**, kartu **Video**:

- **Codec video**: *Otomatis* (H.264 bila tersedia, lalu VP9, VP8, AV1), atau pilihan tetap. Perubahan berlaku pada sesi berikutnya.
- **Akselerasi hardware**: *Otomatis* menyerahkan pilihan ke mesin browser, *Selalu gunakan GPU* meminta GPU dipakai walau masuk daftar blokir, *Nonaktif* memaksa encoding dan decoding software. Perubahan ini memerlukan aplikasi dimulai ulang.

Viewer menampilkan codec yang benar-benar dipakai, misalnya `H.264, hardware`, sehingga hasil pengaturan terlihat.

## Berjalan di latar belakang (Windows)

Di **Pengaturan**, kartu **Latar belakang**:

- **Tetap aktif di system tray** (aktif secara default): saat akses jarak jauh menyala, menekan X atau meminimalkan hanya menyembunyikan jendela. ID tetap online. Klik ikon tray untuk membuka lagi; klik kanan lalu **Keluar** untuk benar-benar menutup. Tooltip ikon menampilkan ID dan status. Bila akses jarak jauh dimatikan, menutup jendela keluar seperti biasa.
- **Jalankan saat Windows dimulai** (opsional): menulis entri `HKCU\...\Run` per pengguna; aplikasi mulai tersembunyi di tray setelah Anda masuk ke Windows. Berlaku setelah Anda login; sebelum itu (layar login) komputer belum bisa dikontrol. Itu butuh Windows Service, ada di [roadmap](docs/ROADMAP.md).
- Menjalankan aplikasi kedua kali tidak membuat host kedua; jendela yang sudah ada yang dimunculkan.
- Dengan persetujuan manual, jendela muncul otomatis saat ada yang meminta izin.

## Memakai proxy

Untuk komputer yang ke internet hanya lewat proxy HTTP, buka **Pengaturan**, kartu **Proxy**:

1. Nyalakan **Pakai proxy HTTP**.
2. Isi alamat dan port proxy, misalnya `10.15.3.20` dan `80`.
3. Tekan **Simpan dan mulai ulang**. Aplikasi mulai ulang untuk menerapkannya.

Catatan:

- Proxy diberikan ke mesin web aplikasi, jadi semua yang dilakukan aplikasi secara online memakainya: koneksi ke server signaling, kredensial relay TURN, dan pemeriksaan pembaruan.
- Hanya proxy HTTP biasa tanpa login yang didukung. Proxy yang meminta username dan kata sandi belum didukung.
- Video berjalan lewat UDP, yang tidak bisa dibawa proxy HTTP. Di balik proxy, sesi harus lewat relay TURN melalui TCP/TLS, dan proxy harus mengizinkan `CONNECT` ke port relay. Cloudflare menyediakan `turns:` di port 443 yang biasanya diizinkan proxy. Bagian ini belum bisa diuji dengan proxy kantor sungguhan.
- Jika Windows sudah punya proxy untuk seluruh sistem, aplikasi biasanya mengikutinya dan pengaturan ini mungkin tidak diperlukan.
- Tidak tercakup proxy ini: tautan yang dibuka di browser (Periksa pembaruan, zivar.my.id); browser memakai pengaturannya sendiri.

## Versi web pribadi

Halaman web untuk mengontrol komputer Anda dari browser mana pun, khusus untuk Anda. Masuknya lewat email: **Cloudflare Access** mengirim kode sekali pakai ke alamat yang Anda daftarkan, dan tidak ada orang lain yang bahkan bisa memuat halamannya. Halaman dilayani oleh Worker tersendiri (`web/`, nama `zivar-web`) yang menjadi satu-satunya jalan browser ke server signaling. App key tidak pernah sampai ke browser.

```
browser --(Access: kode email)--> zivar-web --(service binding + WEB_KEY)--> zivar-signaling
```

Yang bisa dilakukan: menyambung ke komputer dengan ID dan kata sandi (layar atau hanya terminal), juga di ponsel. Yang tidak ada: menjadi komputer yang dikontrol, mengingat kata sandi (di browser tidak ada perlindungan untuknya), menyambung lewat alamat IP lokal (halaman https tidak bisa membuka `ws://`), dan memeriksa pembaruan.

Penyiapan sekali saja:

1. **Rahasia bersama** antara kedua Worker (nilainya sama di keduanya). Di PowerShell:

   ```powershell
   $key = node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
   cd signaling; $key | npx wrangler secret put WEB_KEY
   cd ../web;    $key | npx wrangler secret put WEB_KEY
   ```

2. **Cloudflare Access** (di dashboard, gratis sampai 50 pengguna):
   - *Zero Trust*: pilih nama tim dan paket Free pada pemakaian pertama. Di *Settings, Authentication* pastikan **One-time PIN** aktif.
   - *Workers & Pages, zivar-web, Settings, Domains & Routes*: pada baris `workers.dev` pilih **Enable Cloudflare Access**, lalu **Manage Cloudflare Access**. Pada kebijakannya, izinkan hanya alamat email Anda (Include, Emails) dan hapus aturan yang mengizinkan semua orang di akun atau domain Anda.
   - Salin **Audience (AUD) tag** aplikasi tersebut (*Zero Trust, Access, Applications, zivar-web*) dan **team domain** Anda (`https://<team>.cloudflareaccess.com`, di *Zero Trust, Settings*).

3. Isi keduanya di `web/wrangler.jsonc` pada `vars`, lalu deploy:

   ```jsonc
   "vars": {
     "TEAM_DOMAIN": "https://<team>.cloudflareaccess.com",
     "POLICY_AUD": "<audience tag>",
     "ALLOWED_EMAILS": "anda@contoh.com"
   }
   ```

   ```powershell
   cd web; npm install; npm run deploy
   ```

4. Buka `https://zivar-web.<akun-anda>.workers.dev`, masukkan email, ketik kode yang diterima, lalu sambungkan.

Sebelum langkah 3 selesai, Worker menjawab 503 untuk semuanya, jadi tidak ada yang terbuka tanpa sengaja. Worker juga memeriksa token Access sendiri (tanda tangan, audience, masa berlaku, dan email di `ALLOWED_EMAILS`), sehingga walau Access tidak sengaja dimatikan, tidak ada yang bisa masuk. Jika Anda ditolak padahal sudah masuk, jalankan `npx wrangler tail` di `web/`: alasannya dicatat (`access denied: ...`).

## Pembaruan

Menu **Tentang** punya tombol **Periksa pembaruan**, dan aplikasi juga memeriksa diam-diam saat dibuka (paling sering tiap 12 jam). Versi baru ditandai titik di tab Tentang; tombol unduh membuka berkas rilis di browser. Aplikasi belum memasang pembaruan sendiri (lihat roadmap).

Aplikasi membaca rilis terbaru dari repo rilis publik langsung di GitHub, lengkap dengan catatan rilis dari `CHANGELOG.id.md`; tombol unduh membuka installer atau APK secara langsung. Tidak ada lagi yang perlu diterbitkan manual. Aplikasi sampai 0.10.x masih membaca manifest dari Worker; agar mereka melihat versi baru, terbitkan sekali:

```powershell
node scripts/publish-update.mjs 0.12.0 "Catatan rilis singkat"
```

## Menjalankan server sendiri (Cloudflare, gratis)

```powershell
cd signaling
npx wrangler login
npx wrangler kv namespace create META            # isi id yang dihasilkan ke wrangler.jsonc
npx wrangler deploy                              # menghasilkan https://zivar-signaling.<akun>.workers.dev
```

TURN (opsional tapi disarankan untuk jaringan kantor): buat TURN key di dashboard Cloudflare (Realtime, lalu TURN), kemudian:

```powershell
npx wrangler secret put TURN_KEY_ID
npx wrangler secret put TURN_KEY_API_TOKEN
npx wrangler secret put APP_KEY                  # opsional: mencegah orang lain memakai Worker Anda
```

Cek relay: buka `<alamat-worker>/health`. `{"ok":true,"turn":"ok"}` berarti TURN aktif (`missing` = secret belum dipasang, `invalid:401` = kunci salah).

Alamat server dan kunci ditanam saat build oleh `scripts/write-defaults.mjs`: alamat dari `ZIVAR_SERVER` (default ke Worker proyek ini) dan kunci dari GitHub secret `APP_KEY`. Keduanya tidak ditampilkan di aplikasi. Untuk mengganti kunci: pasang nilai baru di Worker (`npx wrangler secret put APP_KEY`) dan di GitHub (`gh secret set APP_KEY`), lalu rilis versi baru. Build yang lebih lama berhenti tersambung, dan aplikasi memberi tahu pengguna agar memasang versi terbaru.

> Kunci di dalam aplikasi yang dibagikan bisa diambil oleh siapa pun yang memegang berkasnya, dan unduhannya publik. Dengan kunci itu orang bisa memakai Worker dan jatah TURN Anda (Cloudflare Realtime TURN gratis sampai jumlah bulanan yang besar); pantau pemakaian di dashboard Cloudflare dan ganti kunci bila ada penyalahgunaan. Komputernya sendiri dilindungi ID dan kata sandinya, bukan kunci ini.

## Keamanan

- Kata sandi tidak pernah dikirim (tantangan HMAC, lihat di atas).
- ID diklaim dengan token rahasia; perangkat lain tidak bisa merebut ID yang sama.
- Lima kata sandi salah mengunci ID itu selama 15 menit untuk IP pengirim.
- Data channel input baru dibuka setelah autentikasi berhasil.
- Kata sandi tersimpan dienkripsi dengan DPAPI Windows.

## Batasan saat ini

- Host hanya Windows. Viewer jalan di Windows dan Android.
- Layar login, layar kunci, dan dialog UAC belum bisa dikontrol (butuh Windows Service).
- Auto-start berjalan setelah Anda login ke Windows, bukan saat boot. Installer belum ditandatangani.
- Layar login setelah restart, sebelum ada yang login, tidak bisa dikontrol: aplikasinya belum berjalan. Komputer yang terkunci bisa, bila layanan terpasang (lihat di atas).
- Android menyimpan kata sandi hanya di penyimpanan privat aplikasi (belum Keystore) dan APK-nya build debug.
- Hitungan percobaan kata sandi ada di memori Durable Object dan reset bila DO di-restart.
- Proxy belum mendukung username/kata sandi dan belum diuji dengan proxy kantor dan relay sungguhan.

Gagasan lanjutan, berurutan, ada di [docs/ROADMAP.md](docs/ROADMAP.md).

## Pengembangan

```powershell
# terminal 1: server signaling
cd signaling; npm install; npm run dev          # http://127.0.0.1:8787
npm test                                         # tes e2e signaling (server harus jalan)

# terminal 2: aplikasi
cd client; npm install; npm run tauri dev
npm test                                         # tes unit (quality, history, update)
```

Rilis: dorong tag `vX.Y.Z`; `.github/workflows/release.yml` membangun installer Windows dan APK Android lalu menerbitkannya. Dengan GitHub secret `RELEASES_TOKEN` (token fine-grained dengan izin *Contents: read and write* pada repo rilis publik) berkasnya diterbitkan di sana; tanpa itu berkasnya tetap di repo private. Repo publik perlu satu commit (misalnya README) sebelum rilis pertama.

## Struktur proyek

```
client/src/            UI dan logika (main.js, history.js, quality.js, update.js), xterm.js di vendor/
web/                   Viewer web pribadi: Worker yang melayani halaman dan meneruskan ke server signaling (di belakang Cloudflare Access)
client/src-tauri/      Rust: config, injeksi input, tray, proxy, relay jaringan lokal (lan.rs), terminal (term.rs), layanan dan helper layar kunci (secure.rs)
client/branding/       Logo
signaling/src/         Worker + Durable Object
scripts/               write-defaults.mjs, publish-update.mjs
docs/ROADMAP.md        Rencana fitur
.github/workflows/     Build dan rilis
```

## Lisensi

Kode ZIVAR sendiri dirilis di bawah [Lisensi MIT](LICENSE). Aplikasi ini dibangun di atas komponen pihak ketiga, masing-masing dengan lisensinya sendiri; lihat [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
