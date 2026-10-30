# Matriks fitur OHLI Social v2.2.0

Status di bawah menjelaskan apa yang disediakan oleh source code. Fitur tetap perlu diuji di dua akun/perangkat nyata sebelum digunakan publik.

## Sudah memiliki alur UI dan endpoint server

- Registrasi/login, sesi token, penyimpanan akun pada server Android, dan hashing kata sandi PBKDF2 untuk akun baru/perubahan kata sandi.
- Chat privat dan fitur dasar lama: teks, lampiran, kontak, pengeditan/penghapusan sesuai implementasi yang tersedia, status pesan dan polling.
- Feed sosial, posting teks/foto/video, audiens publik/pengikut/hanya saya, like, komentar, simpan, repost, bagikan, edit/hapus, arsip dan pemulihan.
- Profil, edit nama/bio/foto/cover, jumlah postingan, daftar pengikut/mengikuti yang dapat dibuka, dan penghitungan tayangan profil (maksimal satu catatan per akun per hari).
- Akun privat dengan permintaan mengikuti, persetujuan/penolakan permintaan, dan kontrol visibilitas daftar pengikut, daftar mengikuti, postingan disukai, serta koleksi tersimpan. Pemeriksaan akses diterapkan juga pada endpoint server.
- Tab profil Disukai, Posting ulang, Disimpan, Media, Sorotan, dan Tentang.
- Story teks/foto/video, viewer unik dan jumlah pemutaran ulang, statistik penonton untuk pemilik, reaksi suka, dan penyimpanan Story ke Sorotan. Sorotan bertahan setelah Story 24 jam berakhir.
- QR kontak akun sendiri, pemindai QR jika browser mendukung `BarcodeDetector`, dan jalur input nomor manual sebagai fallback.
- Penggantian kata sandi dengan verifikasi kata sandi lama; sesi perangkat lain dikeluarkan setelah kata sandi berubah.
- Pembuatan grup/Channel, alur gabung/keluar atau ikuti/berhenti mengikuti, chat grup teks, serta posting pengumuman teks oleh pemilik Channel.
- LIVE WebRTC peer-to-peer dengan signaling, komentar dan daftar siaran; batas kapasitas nyata tergantung koneksi dan browser.
- UI responsif untuk webview/browser, feed, video pendek, chat, pengaturan, notifikasi, dan navigasi OHLI yang telah ada pada v1.9.

## Ada, tetapi masih terbatas

- Privasi akun menyaring data dan konten melalui API. Jumlah pengikut/mengikuti tetap dapat tampak sebagai angka meski daftar namanya dibatasi.
- Tayangan profil dihitung per akun per hari, bukan setiap refresh. Ini bukan analytics waktu nyata atau daftar kunjungan historis tanpa batas.
- QR scanner menggunakan `BarcodeDetector` atau ZXing browser dan meminta kamera belakang. Kamera browser memerlukan secure context: HTTPS atau localhost; `http://IP-lokal` biasa akan ditolak browser. Jika izin/API tidak tersedia atau library tidak bisa dimuat, input nomor manual menjadi fallback.
- Chat grup dan Channel pada v2.1 hanya mendukung teks; belum ada lampiran, balasan pesan, admin granular, undangan, panggilan grup, ataupun edit/hapus pesan grup.
- Sorotan dibuat dari Story aktif milik sendiri. Belum ada editor urutan, sampul, kategori, atau pengelolaan highlight lanjutan.
- Suka Story berupa tombol toggle dan jumlah suka; belum ada layar daftar orang yang menyukai Story.
- Perbaikan CSS iPhone/WebView mengurangi beberapa kondisi layout/scroll yang diketahui, tetapi layar hitam di iPhone belum dapat dikonfirmasi selesai tanpa reproduksi di perangkat asli.
- LIVE hanya signaling P2P. Untuk banyak penonton diperlukan TURN/SFU; host harus tetap terhubung dan memberi izin kamera/mikrofon.
- Seluruh JSON akun, pesan, status, follow, dan blokir berada di `getFilesDir()` pada perangkat host, dilengkapi file sementara dan cadangan `.bak` untuk pemulihan saat proses mati/listrik terputus. Data bertahan saat reboot, server mati, Wi-Fi terputus, dan alamat IP berubah; setelah IP berubah, buka alamat server baru. Menghapus data aplikasi atau uninstall tetap menghapus data lokal.

## Belum disediakan / perlu infrastruktur atau pengembangan lanjutan

- Database relasional produksi, transaksi multi-proses, indeks pencarian, object storage eksternal, backup off-device, monitoring, WAF/rate limit, audit keamanan, dan pemulihan bencana.
- OTP/SMS/e-mail, reset kata sandi terverifikasi, push notification ketika aplikasi tertutup, dan enkripsi end-to-end dengan protokol yang diaudit.
- TURN/SFU untuk LIVE skala besar, moderasi/laporan, role admin/moderator granular, chat grup dengan media, polling, carousel multi-foto, drafts server-side, hashtag/search terindeks, dan analytics kreator.
- Pengujian dua perangkat fisik (terutama iPhone) dan build APK final. Build lokal pada sesi pembuatan source tidak tersedia karena Android SDK dan Gradle tidak terpasang; gunakan GitHub Actions pada repository.

Jangan menganggap keterbatasan di atas sebagai fitur produksi yang telah selesai.


## Perbaikan v2.1
- Responsif HP: overflow horizontal profil ditahan di viewport dan rail Video Pendek dirapatkan.
- Story satu bar per akun; cerita terbaru terbuka pertama dan cerita sebelumnya dapat dinavigasi.
- Foto profil dapat dilihat ukuran besar, follow state ditampilkan dengan benar, dan double-tap like memakai satu request.
- Centang pesan mengikuti status dikirim/diterima/dibaca; edit/hapus tersedia melalui tekan lama.
- FYP menggunakan skor engagement dan peluruhan waktu; Jelajah Topik menampilkan tagar aktif dari postingan dua minggu terakhir.
- Blokir disimpan di server dan operasi blokir bersifat idempoten.
- JSON menyimpan cadangan atomik di storage internal aplikasi untuk pemulihan setelah restart atau kehilangan daya.


## v2.2.0 additions and limits
| Area | Included in source | Boundary
|---|---|---|
| Mobile social UI | Zoom, overflow guards, cleaner action rail, touch/focus regression fixes, progressive Reels feed pagination | Device-specific browser/WebView verification still required |
| Story | Per-account rings, seen state, autoplay/progress, tap/swipe navigation, draft load from own Media tab | Drafts remain in local IndexedDB; story video is muted until user enables sound |
| Audio/reels | Local audio upload, re-use visible source audio, detail page, related posts, server-saved favorites | Autoplay may be blocked until user gesture |
| Discovery | Engagement/recency FYP score and trending hashtags | Heuristic only; not ML and not guaranteed virality |
| Chat mobile | Multi-file attachment, VN recording, speed controls, favorite stickers, archive menu visible from mobile menu | Browser microphone/camera permissions and API availability apply |
| Theme | Existing presets and custom palette controls | Palette is saved per browser/device |
| QR | Camera scanner, barcode detection, image scan fallback, manual number fallback | Live camera needs HTTPS or localhost + permission |


## v2.2.0 patch notes from mobile bug review
- Video Reels requests feed pages incrementally, skips photo/text-only pages while looking for videos, and exposes a manual load-more action if the next page has no video.
- Browser-side post and Story upload size checks match the Android endpoint limit of 1 GB per file. Free storage and network reliability remain practical limits.
- Story video playback starts muted; a visible sound control allows an explicit user gesture to unmute. Story camera shortcut uses the browser file capture hint.
- The camera QR scanner still requires an HTTPS/localhost secure context and an explicit camera permission. The mobile app cannot bypass browser security restrictions.
- Local music files can be uploaded as audio tracks and used on a post; original video sounds have detail/uses/favorite support. Audio playback depends on browser autoplay permission.
- Theme color pickers update accent, secondary, surface, text and page background colors. Theme settings are saved on the current browser/device.
