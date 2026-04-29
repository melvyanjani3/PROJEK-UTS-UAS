# Software Requirements Specification for TailoriX
**Platform Digital Jasa Permak & Alterasi Pakaian**

**Version 1.0 Approved**  
Prepared by Tim Pengembang TailoriX  
Mata Kuliah: Rekayasa Perangkat Lunak  
April 2025

---
| Nama |
|------|
|Muhammad Ilyas Fauzi  |
|Julia Desteny Deodonia Langkedeng |
|Nazwa Ima Fadia|
|Gilang Anugrah |
|muhammad rifqy wildan|
|Wahyu Bonita Juliana Sari |
|Aal Maulana |
|Caryksha Aulia Putri |

**PROGRAM STUDI SISTEM INFORMASI**
**FAKULTAS ILMU KOMPUTER DAN SISTEM INFORMASI**
**UNIVERSITAS KEBANGSAAN REPUBLIK INDONESIA**
**TAHUN 2026**
## Revision History

| Name | Date | Reason For Changes | Version |
|------|------|-------------------|---------|
| Tim TailoriX | April 2025 | Initial release – dokumen SRS TailoriX v1.0 | 1.0 |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Overall Description](#2-overall-description)
3. [External Interface Requirements](#3-external-interface-requirements)
4. [System Features](#4-system-features)
5. [Other Nonfunctional Requirements](#5-other-nonfunctional-requirements)
6. [Other Requirements](#6-other-requirements)
7. [API Contract](#7-api-contract-desain-kontrak-api)
8. [Appendices](#appendices)

---

## 1. Introduction

### 1.1 Purpose

Dokumen Software Requirements Specification (SRS) ini mendeskripsikan spesifikasi kebutuhan perangkat lunak untuk aplikasi **TailoriX** – Platform Digital Jasa Permak & Alterasi Pakaian. Dokumen ini mencakup deskripsi fungsionalitas, antarmuka pengguna, kebutuhan non-fungsional, dan spesifikasi teknis sistem.

SRS ini disusun sebagai bagian dari mata kuliah Rekayasa Perangkat Lunak. Platform ini dibangun untuk menghubungkan pelanggan dengan penjahit mitra UMKM secara digital, transparan, dan terukur.

### 1.2 Document Conventions

Dokumen ini mengikuti konvensi penulisan sebagai berikut:

- **Heading 1 (H1)**: Judul bab utama, dicetak tebal, ukuran 14pt, warna biru tua.
- **Heading 2 (H2)**: Sub-bab, dicetak tebal, ukuran 12pt, warna biru.
- **Heading 3 (H3)**: Sub-sub-bab, dicetak tebal, ukuran 11pt, hitam.
- **ID Fitur**: Ditulis dengan format `F-XX` (contoh: F-01, F-02).
- **ID Requirement**: Ditulis dengan format `REQ-XX-YY` (contoh: REQ-01-01).
- **Kode API Endpoint**: Ditulis dengan format monospace (contoh: `/api/v1/auth/login`).
- **Prioritas fitur**: High (H), Medium (M), Low (L).

### 1.3 Intended Audience and Reading Suggestions

Dokumen SRS ini ditujukan untuk beberapa kelompok pembaca:

- **Developer & Software Engineer**: Fokus pada Bab 2 (Deskripsi Umum), Bab 3 (Kebutuhan Fungsional), Bab 4 (Antarmuka Eksternal), Bab 5 (Fitur Sistem), dan Bab 7 (Kontrak API).
- **Project Manager & Tim Akademik**: Fokus pada Bab 1 (Pendahuluan), Bab 2 (Deskripsi Umum), dan Bab 6 (Kebutuhan Nonfungsional).
- **System Analyst & Architect**: Fokus pada Bab 4 (Antarmuka), Bab 5 (Fitur Sistem), dan lampiran UML.
- **Tester & QA**: Fokus pada kebutuhan fungsional di Bab 5 dan atribut kualitas di Bab 6.

Disarankan untuk membaca dokumen secara berurutan dari Bab 1 hingga Bab 7, kemudian menelaah lampiran sesuai kebutuhan masing-masing peran.

### 1.4 Product Scope

TailoriX adalah platform digital berbasis mobile yang menghubungkan pelanggan dengan penjahit mitra UMKM penyedia jasa permak, alterasi, dan perbaikan pakaian. Platform ini dirancang untuk menjawab permasalahan kesulitan menemukan jasa permak berkualitas dan terpercaya di era digital.

**Manfaat utama platform ini meliputi:**

- Kemudahan akses layanan permak baju melalui smartphone (Android & iOS).
- Transparansi harga dengan estimasi otomatis berbasis Machine Learning.
- Tracking status pesanan secara real-time.
- Perluasan jangkauan usaha bagi penjahit mitra UMKM.
- Efisiensi proses bisnis melalui otomasi notifikasi (n8n).

**Platform TailoriX TIDAK mencakup:**
- Pembuatan pakaian baru dari awal
- Integrasi logistik pihak ketiga (v1.0)
- Fitur COD
- Versi web untuk pelanggan (v1.0)
- Dukungan multi-bahasa (v1.0 hanya Bahasa Indonesia)

### 1.5 References

- IEEE Std 830-1998, IEEE Recommended Practice for Software Requirements Specifications.
- Karl E. Wiegers, Software Requirements Specification Template, 1999.
- Dokumentasi Flutter 3.x – https://flutter.dev/docs
- Dokumentasi Laravel 11 – https://laravel.com/docs/11.x
- Midtrans Payment Gateway API Documentation – https://api.midtrans.com
- n8n Workflow Automation Documentation – https://docs.n8n.io
- MinIO Object Storage Documentation – https://min.io/docs

---

## 2. Overall Description

### 2.1 Product Perspective

TailoriX merupakan platform baru yang berdiri sendiri (new, self-contained product), bukan merupakan bagian dari sistem yang lebih besar. Platform ini terdiri dari dua aplikasi mobile (untuk pelanggan dan penjahit) dan satu backend API yang terpusat, dilengkapi dengan layanan ML untuk estimasi harga.

Sistem TailoriX berinteraksi dengan komponen eksternal berikut:
- Midtrans Payment Gateway (pemrosesan pembayaran)
- SendGrid (notifikasi email)
- Twilio (notifikasi WhatsApp)
- Google OAuth (autentikasi pihak ketiga)
- MinIO Object Storage (penyimpanan foto)

### 2.2 Product Functions

Fungsi-fungsi utama yang harus disediakan oleh TailoriX:

- **F-01**: Autentikasi & Manajemen Akun – registrasi, login, kelola profil.
- **F-02**: Pencarian & Pemilihan Penjahit – cari berdasarkan lokasi, filter, profil.
- **F-03**: Pemesanan Jasa Permak – buat pesanan dengan foto dan deskripsi.
- **F-04**: Estimasi Harga Otomatis (ML) – analisis foto dan berikan estimasi harga.
- **F-05**: Pembayaran Digital – integrasi Midtrans, DP, dan pelunasan.
- **F-06**: Tracking Status Pesanan – pantau status real-time dan notifikasi.
- **F-07**: Ulasan & Rating – sistem review dua arah setelah pesanan selesai.

### 2.3 User Classes and Characteristics

Terdapat tiga kelas pengguna utama dalam sistem TailoriX:

#### Pelanggan (Customer)
- Pengguna umum yang mencari jasa permak baju.
- Tingkat keahlian teknis beragam; sebagian besar pengguna smartphone dengan literasi digital menengah.
- Frekuensi penggunaan: insidental (saat butuh permak).
- Merupakan kelas pengguna paling penting untuk kepuasan.

#### Penjahit Mitra (Tailor)
- Penyedia jasa permak yang mendaftar sebagai mitra.
- Tingkat keahlian teknis rendah hingga menengah; memerlukan antarmuka yang sederhana.
- Frekuensi penggunaan: harian (menerima dan mengelola pesanan).

#### Admin Platform
- Pengelola platform TailoriX.
- Tingkat keahlian teknis tinggi.
- Mengelola moderasi konten, verifikasi mitra, dan laporan analitik.

### 2.4 Operating Environment

Lingkungan operasional sistem TailoriX:

| Komponen | Spesifikasi |
|----------|-------------|
| Mobile App | Android 8.0 (API Level 26)+; iOS 13.0+; Flutter 3.x |
| Backend API | Ubuntu 22.04 LTS, PHP 8.3, Laravel 11, MySQL 8.0, Redis 7 |
| ML Service | Python 3.11, FastAPI, TensorFlow/PyTorch, Docker container |
| Workflow Engine | n8n self-hosted dalam Docker container |
| Object Storage | MinIO (S3-compatible) dalam Docker container |
| Deployment | Docker Compose (dev), Docker Swarm/Kubernetes (production) |
| Koneksi | Memerlukan koneksi internet aktif untuk semua fitur utama |

### 2.5 Design and Implementation Constraints

- Platform mobile hanya Android dan iOS menggunakan Flutter; tidak ada versi web untuk pelanggan pada v1.0.
- Pembayaran hanya melalui Midtrans; COD tidak didukung pada v1.0.
- Layanan pickup/delivery hanya dalam radius yang ditentukan penjahit; tidak ada integrasi logistik pihak ketiga.
- Fitur ML estimasi harga memerlukan koneksi internet aktif; tidak berjalan offline.
- Data pengguna disimpan di server Indonesia untuk memenuhi regulasi UU PDP (Perlindungan Data Pribadi).
- Bahasa antarmuka hanya Bahasa Indonesia pada v1.0.
- Semua komunikasi antar layanan menggunakan HTTPS dengan enkripsi TLS 1.2+.

### 2.6 User Documentation

- In-app onboarding guide: tutorial interaktif saat pertama kali membuka aplikasi.
- FAQ & Help Center: tersedia dalam aplikasi, dapat diakses melalui menu profil.
- Push notification guide: panduan singkat tentang pengaturan notifikasi.
- Panduan Penjahit Mitra: dokumen PDF yang dikirim saat onboarding mitra penjahit.

### 2.7 Assumptions and Dependencies

#### Asumsi:
- Pengguna memiliki smartphone dengan kamera yang berfungsi untuk mengambil/mengunggah foto pakaian.
- Penjahit mitra memiliki koneksi internet stabil untuk menerima dan mengelola pesanan.
- Model ML awal akan dilatih dengan dataset dummy; akurasi estimasi akan meningkat seiring bertambahnya data feedback.

#### Dependensi:
- **Midtrans**: ketersediaan layanan payment gateway. Gangguan Midtrans akan menghambat proses pembayaran.
- **Google OAuth**: digunakan untuk opsi login Google. Gangguan akan menonaktifkan fitur login Google.
- **Twilio/SendGrid**: digunakan untuk notifikasi. Gangguan akan menghambat pengiriman notifikasi.
- **MinIO**: penyimpanan foto. Gangguan akan menghambat upload dan akses foto pakaian.

---

## 3. External Interface Requirements

### 3.1 User Interfaces

Antarmuka pengguna TailoriX dirancang menggunakan Flutter dengan mengacu pada Material Design 3 dan prinsip Human Interface Guidelines (HIG) untuk iOS. Karakteristik utama:

- Navigasi utama menggunakan Bottom Navigation Bar dengan maksimal 5 item.
- Setiap layar memiliki tombol aksi utama yang jelas dan konsisten.
- Layar pemuatan menggunakan skeleton loading (bukan spinner) untuk pengalaman yang lebih baik.
- Pesan error ditampilkan secara inline di dekat input yang bermasalah.
- Dukungan dark mode mengikuti preferensi sistem operasi.
- Ukuran tap target minimum 48x48dp sesuai standar aksesibilitas.

**Layar utama yang diperlukan:**
Splash/Onboarding, Login/Register, Beranda, Cari Penjahit, Buat Pesanan, Detail Pesanan, Pembayaran, Riwayat Pesanan, Profil, dan Admin Panel (web).

### 3.2 Hardware Interfaces

- **Kamera**: akses kamera dan galeri foto untuk upload foto pakaian (menggunakan `flutter_image_picker`).
- **GPS/Lokasi**: akses lokasi perangkat untuk fitur pencarian penjahit berdasarkan jarak (menggunakan `geolocator`).
- **Storage**: akses penyimpanan lokal untuk cache gambar dan token autentikasi (`flutter_secure_storage`).
- **Push Notification**: akses Firebase Cloud Messaging (FCM) untuk notifikasi push.
- **Biometrik**: opsional, fingerprint/face ID untuk login cepat.

### 3.3 Software Interfaces

| Komponen | Versi | Fungsi | Protokol |
|----------|-------|--------|----------|
| Laravel API | 11.x | Business logic & REST API backend | HTTPS REST JSON |
| MySQL | 8.0 | Database relasional utama | MySQL Protocol |
| Redis | 7.x | Cache, session, job queue | Redis Protocol |
| ML Service (FastAPI) | Python 3.11 | Estimasi harga via Computer Vision | HTTP JSON (internal) |
| Midtrans Snap API | v2 | Payment gateway | HTTPS REST JSON |
| n8n | latest | Otomasi notifikasi & workflow | Webhook HTTP |
| MinIO (S3) | latest | Object storage foto & file | S3 SDK / HTTPS |
| Firebase FCM | v9 | Push notification mobile | HTTPS |
| Google OAuth 2.0 | v2 | Autentikasi Google login | OAuth 2.0 HTTPS |

### 3.4 Communications Interfaces

- **REST API**: semua komunikasi antara Mobile App dan Backend menggunakan HTTPS (TLS 1.2+), format JSON, dengan JWT Bearer Token di header Authorization.
- **Webhook Midtrans**: HTTPS POST callback dari Midtrans ke endpoint `/api/v1/payments/webhook` saat pembayaran berhasil/gagal.
- **Webhook n8n**: HTTPS POST dari Laravel API ke n8n saat event bisnis terjadi (pesanan baru, pembayaran sukses, perubahan status).
- **Email (SendGrid)**: protokol SMTP via SendGrid API untuk notifikasi email transaksional.
- **WhatsApp (Twilio)**: Twilio WhatsApp Business API untuk notifikasi via WhatsApp.
- **FCM (Firebase)**: push notification real-time ke perangkat mobile melalui Firebase Cloud Messaging.
- **Transfer rate**: tidak ada persyaratan khusus; koneksi 4G/LTE atau Wi-Fi sudah memadai.

---

## 4. System Features

Bagian ini mendefinisikan fitur-fitur fungsional sistem TailoriX secara detail, termasuk deskripsi, prioritas, alur stimulus/respons, dan kebutuhan fungsional spesifik.

### 4.1 F-01: Autentikasi & Manajemen Akun

#### 4.1.1 Description and Priority
Fitur ini memungkinkan pengguna (pelanggan, penjahit, dan admin) untuk mendaftar, masuk, mengelola profil, dan mengakhiri sesi di aplikasi TailoriX.  
**Prioritas: High**

#### 4.1.2 Stimulus/Response Sequences
- Pengguna membuka aplikasi → tampilkan layar login/register → pengguna mengisi form registrasi → sistem kirim OTP → pengguna verifikasi → akun aktif, masuk ke beranda.
- Pengguna memilih login Google → sistem redirect ke Google OAuth → Google return token → sistem buat/update akun → pengguna masuk beranda.
- Pengguna lupa password → klik 'Lupa Password' → isi email → sistem kirim link reset → pengguna klik link → isi password baru → password berhasil diubah.

#### 4.1.3 Functional Requirements
- **REQ-01-01**: Sistem harus mendukung registrasi dengan email atau nomor HP dan verifikasi OTP 6 digit.
- **REQ-01-02**: Sistem harus mendukung login dengan email/password dan Google OAuth 2.0.
- **REQ-01-03**: Pengguna dapat mengelola profil (nama, foto, alamat, nomor HP).
- **REQ-01-04**: Sistem harus mendukung reset password melalui link yang dikirim ke email.
- **REQ-01-05**: Sistem harus mendukung logout yang menginvalidasi JWT token di server.
- **REQ-01-06**: Pengguna dapat menghapus akun; data terkait dianonimkan sesuai kebijakan privasi.
- **REQ-01-07**: Token JWT memiliki masa berlaku 1 jam; refresh token berlaku 30 hari.

### 4.2 F-02: Pencarian & Pemilihan Penjahit

#### 4.2.1 Description and Priority
Fitur ini memungkinkan pelanggan mencari dan memilih penjahit mitra berdasarkan berbagai parameter seperti lokasi, kategori jasa, rating, dan harga.  
**Prioritas: High**

#### 4.2.2 Stimulus/Response Sequences
- Pelanggan buka halaman pencarian → izinkan akses lokasi → sistem tampilkan daftar penjahit terdekat → pelanggan terapkan filter → sistem perbarui daftar → pelanggan pilih penjahit → tampil profil lengkap.
- Pelanggan pilih tampilan map → tampil peta dengan pin penjahit → pelanggan tap pin → tampil info singkat → pelanggan tap 'Lihat Profil' → tampil profil lengkap.

#### 4.2.3 Functional Requirements
- **REQ-02-01**: Sistem menampilkan daftar penjahit berdasarkan jarak dari lokasi pengguna (radius dapat disesuaikan).
- **REQ-02-02**: Sistem mendukung filter: kategori jasa, rating minimum, dan rentang harga.
- **REQ-02-03**: Profil penjahit menampilkan: nama toko, foto, spesialisasi, rating agregat, ulasan, portofolio, dan status ketersediaan.
- **REQ-02-04**: Sistem mendukung tampilan peta (map view) dengan marker lokasi penjahit.
- **REQ-02-05**: Pelanggan dapat menambahkan/menghapus penjahit dari daftar favorit.
- **REQ-02-06**: Hasil pencarian dipaginasi dengan maksimal 20 item per halaman.

### 4.3 F-03: Pemesanan Jasa Permak

#### 4.3.1 Description and Priority
Fitur ini memungkinkan pelanggan membuat pesanan jasa permak dengan mengunggah foto pakaian, mendeskripsikan kebutuhan, dan memilih opsi pengiriman.  
**Prioritas: High**

#### 4.3.2 Stimulus/Response Sequences
- Pelanggan pilih penjahit → klik 'Pesan Sekarang' → isi form pesanan (foto, deskripsi, kategori, deadline) → sistem kirim ke ML Service untuk estimasi harga → tampilkan estimasi → pelanggan konfirmasi → pesanan dibuat, notifikasi dikirim ke penjahit.
- Penjahit terima notifikasi pesanan baru → buka detail pesanan → terima/tolak → jika terima, isi harga final → konfirmasi → pelanggan menerima notifikasi persetujuan.

#### 4.3.3 Functional Requirements
- **REQ-03-01**: Pelanggan dapat mengunggah maksimal 5 foto pakaian (format JPG/PNG, maks 5MB per foto).
- **REQ-03-02**: Pelanggan mengisi deskripsi kebutuhan permak dalam teks bebas (maks 500 karakter).
- **REQ-03-03**: Pelanggan memilih kategori jasa: ubah ukuran, ganti ritsleting, tambal, sulam, atau lainnya.
- **REQ-03-04**: Pelanggan dapat menentukan deadline pengerjaan dengan pemilih tanggal.
- **REQ-03-05**: Pelanggan memilih mode pengiriman: antar ke toko atau pickup oleh kurir mitra penjahit.
- **REQ-03-06**: Penjahit menerima notifikasi pesanan baru dan dapat menerima/menolak dalam 24 jam.
- **REQ-03-07**: Penjahit dapat menyesuaikan harga final (maks 20% di atas estimasi ML tanpa alasan; di atas 20% harus disertai catatan).

### 4.4 F-04: Estimasi Harga Otomatis (ML)

#### 4.4.1 Description and Priority
Fitur ini menggunakan model Machine Learning berbasis Computer Vision untuk menganalisis foto pakaian dan memberikan estimasi harga otomatis kepada pelanggan dan penjahit.  
**Prioritas: Medium-High**

#### 4.4.2 Stimulus/Response Sequences
- Pelanggan upload foto & isi deskripsi → sistem kirim data ke ML Service → ML Service analisis gambar & kategori → return estimasi harga (min-max) beserta confidence level → tampilkan ke pelanggan.

#### 4.4.3 Functional Requirements
- **REQ-04-01**: ML Service menganalisis foto pakaian menggunakan model Computer Vision untuk mengidentifikasi jenis pakaian dan tingkat kerusakan/perubahan yang diperlukan.
- **REQ-04-02**: Sistem menampilkan estimasi harga dalam rentang (min-max) beserta confidence level kepada pelanggan.
- **REQ-04-03**: Penjahit dapat menyesuaikan harga final dari estimasi ML.
- **REQ-04-04**: Sistem menyimpan riwayat estimasi dan harga final untuk retraining model.
- **REQ-04-05**: Waktu respons estimasi ML tidak boleh melebihi 10 detik dalam kondisi normal.
- **REQ-04-06**: Jika ML Service tidak tersedia, sistem menampilkan pesan error dan meminta harga manual dari penjahit.

### 4.5 F-05: Pembayaran Digital

#### 4.5.1 Description and Priority
Fitur ini memungkinkan pelanggan melakukan pembayaran secara digital melalui berbagai metode yang terintegrasi dengan Midtrans payment gateway, termasuk sistem DP dan pelunasan.  
**Prioritas: High**

#### 4.5.2 Stimulus/Response Sequences
- Penjahit terima pesanan → pelanggan diarahkan ke halaman pembayaran → pilih metode → Midtrans tampilkan payment page → pelanggan selesaikan pembayaran → Midtrans kirim webhook → sistem update status → konfirmasi ke kedua belah pihak.

#### 4.5.3 Functional Requirements
- **REQ-05-01**: Sistem mengintegrasikan Midtrans Snap API untuk pemrosesan pembayaran.
- **REQ-05-02**: Metode pembayaran yang didukung: transfer bank (BCA, BNI, BRI, Mandiri), e-wallet (GoPay, OVO, DANA), dan virtual account.
- **REQ-05-03**: Sistem mendukung pembayaran DP (50% dari harga final) dan pelunasan setelah pesanan selesai.
- **REQ-05-04**: Sistem menghasilkan invoice digital dan bukti pembayaran yang dapat diunduh.
- **REQ-05-05**: Sistem memproses refund otomatis jika pesanan dibatalkan sebelum penjahit memulai pengerjaan.
- **REQ-05-06**: Pembayaran harus dikonfirmasi dalam 24 jam setelah order dikonfirmasi penjahit; jika tidak, order otomatis dibatalkan.

### 4.6 F-06: Tracking Status Pesanan

#### 4.6.1 Description and Priority
Fitur ini memungkinkan pelanggan dan penjahit memantau status pesanan secara real-time, berkomunikasi melalui in-app chat, dan menerima notifikasi otomatis setiap perubahan status.  
**Prioritas: High**

#### 4.6.2 Stimulus/Response Sequences
- Penjahit update status pesanan → sistem catat timestamp & kirim event ke n8n → n8n trigger push notification ke pelanggan & email/WhatsApp → pelanggan lihat update di timeline pesanan.

#### 4.6.3 Functional Requirements
- **REQ-06-01**: Status pesanan terdiri dari 5 tahap: Menunggu Konfirmasi → Dikonfirmasi → Proses Permak → Siap Diambil → Selesai.
- **REQ-06-02**: Setiap perubahan status memicu push notification real-time ke pelanggan.
- **REQ-06-03**: Sistem menampilkan timeline visual progres pengerjaan dengan timestamp setiap tahap.
- **REQ-06-04**: Pelanggan dapat menghubungi penjahit melalui in-app chat (teks dan foto).
- **REQ-06-05**: n8n mengirimkan notifikasi otomatis via email (SendGrid) dan WhatsApp (Twilio) untuk setiap perubahan status penting.
- **REQ-06-06**: Riwayat chat disimpan selama 90 hari setelah pesanan selesai.

### 4.7 F-07: Ulasan & Rating

#### 4.7.1 Description and Priority
Fitur ini memungkinkan pelanggan memberikan ulasan dan rating kepada penjahit setelah pesanan selesai, serta memungkinkan penjahit membalas ulasan tersebut.  
**Prioritas: Medium**

#### 4.7.2 Stimulus/Response Sequences
- Pesanan berstatus 'Selesai' → sistem kirim push notification ke pelanggan untuk memberikan ulasan → pelanggan isi rating (1-5 bintang) & teks ulasan & foto opsional → submit → ulasan tampil di profil penjahit.

#### 4.7.3 Functional Requirements
- **REQ-07-01**: Pelanggan dapat memberikan rating bintang 1-5 dan ulasan teks (maks 300 karakter) setelah pesanan selesai.
- **REQ-07-02**: Pelanggan dapat mengunggah foto hasil permak bersamaan dengan ulasan (maks 3 foto).
- **REQ-07-03**: Penjahit dapat memberikan balasan ulasan satu kali (maks 200 karakter).
- **REQ-07-04**: Sistem menghitung rating agregat penjahit berdasarkan rata-rata semua ulasan.
- **REQ-07-05**: Admin dapat memoderasi ulasan yang dilaporkan; ulasan yang melanggar kebijakan dapat disembunyikan.
- **REQ-07-06**: Ulasan dapat diedit oleh pelanggan dalam waktu 7 hari setelah dikirim.

---

## 5. Other Nonfunctional Requirements

### 5.1 Performance Requirements

- Waktu respons API untuk operasi CRUD standar: ≤ 500ms pada beban normal (< 1000 concurrent users).
- Waktu respons estimasi ML: ≤ 10 detik per request.
- Throughput sistem: minimal 100 transaksi per menit pada kondisi puncak.
- Waktu loading halaman utama aplikasi mobile: ≤ 2 detik pada koneksi 4G.
- Database query time: ≤ 200ms untuk query yang tidak melibatkan ML.
- Uptime sistem: 99.5% per bulan (downtime maksimal ~3.6 jam/bulan).

### 5.2 Safety Requirements

- Sistem harus mencegah double-payment: setiap order hanya dapat diproses satu transaksi aktif pada satu waktu.
- Sistem harus memiliki mekanisme rollback otomatis jika transaksi pembayaran gagal di tengah proses.
- Data foto pakaian tidak boleh digunakan untuk tujuan selain estimasi harga dan portofolio penjahit.
- Sistem harus memiliki logging audit trail untuk semua perubahan status pesanan dan transaksi keuangan.

### 5.3 Security Requirements

- Semua komunikasi menggunakan HTTPS dengan TLS 1.2 atau lebih tinggi.
- Password disimpan menggunakan bcrypt dengan salt (cost factor minimal 12).
- JWT token menggunakan algoritma RS256; private key disimpan secara aman di server.
- API dilindungi dengan rate limiting: maksimal 60 request per menit per IP untuk endpoint publik; 120 untuk endpoint terautentikasi.
- Data sensitif pengguna (nomor HP, alamat) dienkripsi di database menggunakan AES-256.
- Webhook Midtrans diverifikasi menggunakan signature key sebelum diproses.
- Autentikasi dua faktor (2FA) opsional untuk pelanggan; wajib untuk admin.
- OWASP Top 10 menjadi acuan dalam pengembangan untuk mencegah SQL Injection, XSS, CSRF, dan vulnerability umum lainnya.

### 5.4 Software Quality Attributes

- **Usability**: antarmuka harus dapat dipelajari oleh pengguna baru dalam maksimal 5 menit; user error rate < 5%.
- **Reliability**: MTBF (Mean Time Between Failures) minimal 720 jam; recovery dari crash < 30 detik.
- **Maintainability**: codebase mengikuti PSR-12 (PHP) dan Dart Style Guide; coverage unit test minimal 70%.
- **Portability**: aplikasi mobile berjalan di Android 8.0+ dan iOS 13.0+ tanpa modifikasi.
- **Scalability**: arsitektur mendukung horizontal scaling; load balancer dapat menambah instance API tanpa downtime.
- **Testability**: setiap modul memiliki unit test dan integration test; CI/CD pipeline berjalan otomatis.

### 5.5 Business Rules

- Penjahit harus terverifikasi (KTP + foto toko) oleh admin sebelum dapat menerima pesanan.
- Komisi platform TailoriX: 10% dari harga final setiap transaksi yang berhasil.
- Pelanggan hanya dapat membatalkan pesanan sebelum penjahit memulai pengerjaan; setelah itu hanya dapat mengajukan dispute.
- Penjahit yang memiliki rating di bawah 3.0 selama 30 hari berturut-turut akan mendapatkan peringatan; di bawah 2.5 selama 60 hari akan dinonaktifkan sementara.
- Dana DP yang dibayarkan tidak dapat di-refund jika pembatalan dilakukan oleh pelanggan setelah penjahit memulai pengerjaan.
- Satu akun email/nomor HP hanya dapat digunakan untuk satu akun (pelanggan ATAU penjahit, tidak keduanya).

---

## 6. Other Requirements

### 6.1 Database Requirements

- Database relasional MySQL 8.0 digunakan untuk menyimpan data utama (user, order, payment, review).
- Redis 7 digunakan untuk caching, session management, dan job queue.
- Foto dan file disimpan di MinIO (S3-compatible object storage), bukan di database relasional.
- Backup database dilakukan setiap hari pukul 02.00 WIB; retensi backup 30 hari.
- Database harus mendukung UTF-8 (utf8mb4) untuk mendukung karakter emoji dan bahasa Indonesia.

### 6.2 Internationalization Requirements

Pada versi 1.0, sistem hanya mendukung Bahasa Indonesia. Arsitektur sistem dirancang dengan mempertimbangkan kemungkinan penambahan bahasa lain di versi mendatang menggunakan Flutter's l10n package dan Laravel localization.

### 6.3 Legal Requirements

- Platform harus mematuhi Undang-Undang Nomor 27 Tahun 2022 tentang Perlindungan Data Pribadi (UU PDP) Indonesia.
- Syarat & Ketentuan serta Kebijakan Privasi harus ditampilkan dan disetujui pengguna saat registrasi.
- Platform harus mematuhi regulasi Bank Indonesia terkait transaksi digital (PBI No.23/6/PBI/2021).

---

## 7. API Contract (Desain Kontrak API)

### 7.1 Konvensi API

- **Base URL**: `https://api.tailorix.id/api/v1`
- **Authentication**: Bearer Token (JWT) pada header Authorization.
- **Content-Type**: `application/json` (request & response).
- **HTTP Status Codes**: 200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 422 Validation Error, 500 Internal Server Error.

**Format Response Standar:**
```json
{
  "status": "success" | "error",
  "message": "string",
  "data": { ... } | null,
  "errors": { ... } | null
}
```

### 7.2 API Endpoints – Authentication (F-01)

#### 7.2.1 POST `/auth/register`
**Deskripsi**: Registrasi pengguna baru dengan email/HP

**Request:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "08123456789",
  "password": "SecurePass123!",
  "user_type": "customer" | "tailor",
  "terms_agreed": true
}
```

**Response (201):**
```json
{
  "status": "success",
  "message": "OTP telah dikirim ke email Anda",
  "data": {
    "user_id": "USR-001",
    "email": "john@example.com",
    "otp_expires_at": "2026-04-29T10:15:00Z"
  }
}
```

#### 7.2.2 POST `/auth/verify-otp`
**Deskripsi**: Verifikasi OTP untuk aktivasi akun

**Request:**
```json
{
  "user_id": "USR-001",
  "otp_code": "123456"
}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Akun berhasil diaktifkan",
  "data": {
    "user_id": "USR-001",
    "access_token": "eyJhbGciOiJSUzI1NiIs...",
    "refresh_token": "eyJhbGciOiJSUzI1NiIs...",
    "expires_in": 3600
  }
}
```

#### 7.2.3 POST `/auth/login`
**Deskripsi**: Login dengan email/password

**Request:**
```json
{
  "email": "john@example.com",
  "password": "SecurePass123!"
}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Login berhasil",
  "data": {
    "user_id": "USR-001",
    "access_token": "eyJhbGciOiJSUzI1NiIs...",
    "refresh_token": "eyJhbGciOiJSUzI1NiIs...",
    "expires_in": 3600
  }
}
```

#### 7.2.4 POST `/auth/logout`
**Deskripsi**: Logout dan invalidasi token

**Request:** Header + Authorization Bearer Token

**Response (200):**
```json
{
  "status": "success",
  "message": "Logout berhasil"
}
```

### 7.3 API Endpoints – Tailor (F-02)

#### 7.3.1 GET `/tailors/search`
**Deskripsi**: Cari penjahit berdasarkan filter

**Query Parameters:**
- `latitude`: float (lokasi pengguna)
- `longitude`: float (lokasi pengguna)
- `radius`: int (dalam km, default 5)
- `category`: string (optional)
- `min_rating`: float (optional, 0-5)
- `page`: int (default 1)
- `limit`: int (default 20)

**Response (200):**
```json
{
  "status": "success",
  "message": "Data penjahit ditemukan",
  "data": {
    "tailors": [
      {
        "tailor_id": "TLR-001",
        "shop_name": "Jahitan Rapi",
        "rating": 4.5,
        "review_count": 120,
        "distance": 2.3,
        "latitude": -6.2088,
        "longitude": 106.8456,
        "categories": ["ubah_ukuran", "ganti_ritsleting"],
        "photo_url": "https://..."
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "total_items": 100
    }
  }
}
```

#### 7.3.2 GET `/tailors/{tailor_id}`
**Deskripsi**: Dapatkan detail profil penjahit

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "tailor_id": "TLR-001",
    "shop_name": "Jahitan Rapi",
    "owner_name": "Budi Santoso",
    "rating": 4.5,
    "review_count": 120,
    "bio": "Penjahit berpengalaman 20 tahun",
    "address": "Jl. Merdeka No. 123",
    "phone": "081234567890",
    "categories": ["ubah_ukuran", "ganti_ritsleting"],
    "portfolio": [
      {
        "photo_url": "https://...",
        "description": "Hasil permak celana"
      }
    ],
    "reviews": [
      {
        "customer_name": "Andi",
        "rating": 5,
        "comment": "Bagus, cepat!",
        "created_at": "2026-04-20T10:00:00Z"
      }
    ]
  }
}
```

### 7.4 API Endpoints – Orders (F-03, F-04)

#### 7.4.1 POST `/orders`
**Deskripsi**: Buat pesanan baru

**Request (multipart/form-data):**
```
tailor_id: "TLR-001"
category: "ubah_ukuran"
description: "Perlu dipanjangin 5cm"
deadline: "2026-05-10"
delivery_mode: "pickup"
photos: [file1.jpg, file2.jpg]
```

**Response (201):**
```json
{
  "status": "success",
  "message": "Pesanan dibuat, menunggu estimasi harga",
  "data": {
    "order_id": "ORD-001",
    "status": "pending_estimation",
    "created_at": "2026-04-29T10:00:00Z"
  }
}
```

#### 7.4.2 GET `/orders/{order_id}/estimate`
**Deskripsi**: Dapatkan estimasi harga dari ML Service

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "order_id": "ORD-001",
    "estimated_price": {
      "min": 50000,
      "max": 100000,
      "currency": "IDR"
    },
    "confidence_level": 0.87,
    "breakdown": {
      "labor": 40000,
      "material": 20000
    }
  }
}
```

#### 7.4.3 GET `/orders/{order_id}`
**Deskripsi**: Dapatkan detail pesanan

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "order_id": "ORD-001",
    "customer_id": "USR-001",
    "tailor_id": "TLR-001",
    "status": "waiting_confirmation",
    "category": "ubah_ukuran",
    "description": "Perlu dipanjangin 5cm",
    "deadline": "2026-05-10",
    "delivery_mode": "pickup",
    "estimated_price": {
      "min": 50000,
      "max": 100000
    },
    "final_price": null,
    "payment_status": "pending",
    "timeline": [
      {
        "status": "waiting_confirmation",
        "timestamp": "2026-04-29T10:00:00Z"
      }
    ],
    "created_at": "2026-04-29T10:00:00Z"
  }
}
```

### 7.5 API Endpoints – Payments (F-05)

#### 7.5.1 POST `/payments/create-transaction`
**Deskripsi**: Buat transaksi pembayaran via Midtrans

**Request:**
```json
{
  "order_id": "ORD-001",
  "amount": 50000,
  "payment_type": "dp" | "full"
}
```

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "transaction_id": "TXN-001",
    "order_id": "ORD-001",
    "amount": 50000,
    "snap_token": "0e4cb67f-4d78-4f40-a0da-0c9c2e9baf97",
    "redirect_url": "https://app.midtrans.com/snap/v2/..."
  }
}
```

#### 7.5.2 POST `/payments/webhook`
**Deskripsi**: Webhook dari Midtrans untuk konfirmasi pembayaran

**Request:**
```json
{
  "transaction_id": "TXN-001",
  "order_id": "ORD-001",
  "status_code": "200",
  "transaction_status": "settlement" | "pending" | "deny"
}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Pembayaran berhasil diproses"
}
```

### 7.6 API Endpoints – Order Status (F-06)

#### 7.6.1 PUT `/orders/{order_id}/status`
**Deskripsi**: Update status pesanan

**Request:**
```json
{
  "status": "confirmed" | "in_progress" | "ready_pickup" | "completed"
}
```

**Response (200):**
```json
{
  "status": "success",
  "message": "Status pesanan diperbarui",
  "data": {
    "order_id": "ORD-001",
    "new_status": "in_progress",
    "updated_at": "2026-04-29T10:15:00Z"
  }
}
```

#### 7.6.2 POST `/orders/{order_id}/messages`
**Deskripsi**: Kirim pesan chat dalam pesanan

**Request (multipart/form-data):**
```
message: "Berapa lama estimasi waktu?"
photo: (optional) file.jpg
```

**Response (201):**
```json
{
  "status": "success",
  "data": {
    "message_id": "MSG-001",
    "sender_id": "USR-001",
    "message": "Berapa lama estimasi waktu?",
    "created_at": "2026-04-29T10:20:00Z"
  }
}
```

### 7.7 API Endpoints – Reviews (F-07)

#### 7.7.1 POST `/reviews`
**Deskripsi**: Buat ulasan untuk penjahit

**Request (multipart/form-data):**
```
order_id: "ORD-001"
rating: 5
comment: "Hasil permak sangat memuaskan!"
photos: [file1.jpg, file2.jpg]
```

**Response (201):**
```json
{
  "status": "success",
  "message": "Ulasan berhasil ditambahkan",
  "data": {
    "review_id": "REV-001",
    "order_id": "ORD-001",
    "tailor_id": "TLR-001",
    "rating": 5,
    "created_at": "2026-04-29T10:30:00Z"
  }
}
```

#### 7.7.2 GET `/tailors/{tailor_id}/reviews`
**Deskripsi**: Dapatkan daftar ulasan penjahit

**Query Parameters:**
- `page`: int (default 1)
- `limit`: int (default 10)

**Response (200):**
```json
{
  "status": "success",
  "data": {
    "tailor_id": "TLR-001",
    "average_rating": 4.5,
    "total_reviews": 120,
    "reviews": [
      {
        "review_id": "REV-001",
        "customer_name": "Andi",
        "rating": 5,
        "comment": "Hasil permak sangat memuaskan!",
        "photos": ["https://..."],
        "created_at": "2026-04-29T10:30:00Z"
      }
    ]
  }
}
```

---

## 8. Appendices

### Glossary & Terminology

| Istilah / Akronim | Definisi |
|---|---|
| **SRS** | Software Requirements Specification – dokumen yang mendeskripsikan kebutuhan perangkat lunak secara lengkap. |
| **TailoriX** | Nama platform digital jasa permak dan alterasi pakaian yang dikembangkan. |
| **Permak Baju** | Layanan perbaikan, perubahan ukuran, atau modifikasi pakaian yang sudah ada. |
| **JWT** | JSON Web Token – standar terbuka untuk transmisi informasi secara aman antar pihak sebagai objek JSON. |
| **ML** | Machine Learning – cabang kecerdasan buatan yang memungkinkan sistem belajar dari data. |
| **n8n** | Platform workflow automation open-source yang digunakan untuk otomasi notifikasi dan proses bisnis. |
| **Midtrans** | Payment gateway gateway Indonesia yang menyediakan berbagai metode pembayaran digital. |
| **MinIO** | Object storage S3-compatible untuk menyimpan file foto dan dokumen. |
| **FCM** | Firebase Cloud Messaging – layanan push notification real-time dari Google. |
| **OTP** | One-Time Password – kode 6 digit yang dikirim untuk verifikasi autentikasi. |
| **HTTPS** | Hypertext Transfer Protocol Secure – protokol komunikasi web yang terenkripsi. |
| **TLS** | Transport Layer Security – protokol enkripsi untuk komunikasi jaringan. |
| **REST API** | Representational State Transfer – arsitektur API yang menggunakan HTTP verbs (GET, POST, PUT, DELETE). |
| **DP** | Down Payment – pembayaran awal/uang muka sebagian dari harga. |
| **Webhook** | Callback HTTP yang dikirim dari sistem eksternal ke sistem kami saat event tertentu terjadi. |
| **Rate Limiting** | Mekanisme pembatasan jumlah request yang diizinkan per periode waktu. |
| **Bcrypt** | Algoritma hashing password yang aman dengan mechanism salting. |
| **AES-256** | Advanced Encryption Standard dengan key 256-bit untuk enkripsi data. |
| **MTBF** | Mean Time Between Failures – rata-rata waktu sistem berjalan sebelum terjadi kegagalan. |
| **UU PDP** | Undang-Undang Perlindungan Data Pribadi Indonesia No. 27 Tahun 2022. |
| **2FA** | Two-Factor Authentication – autentikasi ganda menggunakan dua metode verifikasi. |
| **OWASP Top 10** | Daftar 10 kerentanan keamanan aplikasi web paling kritis versi OWASP. |
| **Virtualisasi** | Teknologi menjalankan multiple sistem operasi atau layanan dalam satu hardware. |
| **Docker** | Platform containerization untuk packaging aplikasi dengan dependency dalam image. |
| **Kubernetes** | Orchestration system untuk automasi deployment, scaling, dan management container. |
| **CI/CD** | Continuous Integration / Continuous Deployment – praktik otomasi build, test, dan deployment. |

---

**Dokumen ini adalah milik Tim Pengembang TailoriX untuk keperluan akademik Mata Kuliah Rekayasa Perangkat Lunak 2026.**
