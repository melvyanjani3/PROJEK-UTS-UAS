# SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
## PermakIn
### Platform Pemesanan Jasa Permak Baju Lokal Berbasis Mobile

**Version:** 1.0 approved  
**Status:** Draft – Evaluasi UTS  
**Tanggal Dibuat:** April 2025  
**Tema:** Tema 2 – Ketahanan Ekonomi / Jasa Lokal  
**Mata Kuliah:** Rekayasa Sistem Informasi – Kelas A2  

**Prepared by:** Tim Kelompok  
**Program Studi:** Sistem Informasi  
**Lokasi:** Bandung, April 2025

---

## Revision History

| Name | Date | Reason For Changes | Version |
|------|------|-------------------|---------|
| Tim Kelompok | April 2025 | Pembuatan dokumen SRS awal untuk evaluasi UTS | 1.0 |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Overall Description](#2-overall-description)
3. [External Interface Requirements](#3-external-interface-requirements)
4. [System Features](#4-system-features)
5. [Other Nonfunctional Requirements](#5-other-nonfunctional-requirements)
6. [System Architecture & API Design](#6-system-architecture--api-design)
7. [Other Requirements](#7-other-requirements)
8. [Appendix A: Glossary](#appendix-a-glossary)
9. [Appendix B: Analysis Models](#appendix-b-analysis-models)
10. [Appendix C: To Be Determined (TBD) List](#appendix-c-to-be-determined-tbd-list)

---

## 1. Introduction

### 1.1 Purpose

Dokumen ini merupakan Software Requirements Specification (SRS) untuk sistem PermakIn versi 1.0. Dokumen ini menetapkan seluruh kebutuhan fungsional dan non-fungsional, arsitektur sistem, serta kontrak API yang menjadi acuan pengembangan platform PermakIn. SRS ini berlaku untuk semua komponen sistem: aplikasi mobile (Flutter), API backend (Laravel), layanan machine learning (Python/FastAPI), dan automation workflow (n8n).

### 1.2 Document Conventions

Dokumen ini menggunakan konvensi penulisan sebagai berikut:

- **Heading bertingkat (H1–H3)** digunakan untuk menandai hierarki bagian dokumen
- **Tabel** digunakan untuk menyajikan kebutuhan fungsional, endpoint API, dan data terstruktur
- **Kode requirement** diberi prefiks: F-xx (Fungsional), NF-xx (Non-Fungsional)
- **Istilah teknis** dalam bahasa Inggris ditulis apa adanya (REST, JWT, Flutter, dsb.)
- **Kata "sistem"** merujuk pada keseluruhan platform PermakIn kecuali disebutkan spesifik
- **Setiap kebutuhan** memiliki prioritas: Tinggi / Sedang / Rendah

### 1.3 Intended Audience and Reading Suggestions

Dokumen SRS ini ditujukan kepada beberapa kelompok pembaca berikut:

- **Tim Pengembang (Developer):** Membaca seluruh dokumen, dengan fokus pada Bab 4 (System Features), Bab 3 (External Interface), dan Bab 5 (Arsitektur & API)
- **Project Manager:** Fokus pada Bab 1 (Introduction), Bab 2 (Overall Description), dan Bab 5 (Nonfunctional Requirements)
- **Penguji / QA Engineer:** Fokus pada Bab 4 (System Features) dan Bab 5 (Performance & Security Requirements)
- **Dosen / Evaluator:** Membaca secara menyeluruh sesuai rubrik penilaian UTS Rekayasa Sistem Informasi
- **Pengguna Akhir (Pelanggan & Penjahit):** Cukup membaca Bab 2 (Product Functions & User Classes)

### 1.4 Product Scope

PermakIn adalah platform mobile berbasis Android (Flutter) yang menghubungkan pelanggan dengan penjahit/tukang permak lokal di Kota Bandung. Sistem memungkinkan pelanggan mencari, memesan, dan memantau layanan alterasi/permak pakaian secara digital; sementara penjahit dapat mengelola profil, layanan, pesanan, dan portofolio melalui dashboard terintegrasi.

Tujuan bisnis utama PermakIn:

- Meningkatkan visibilitas dan jangkauan pasar penjahit lokal melalui teknologi digital
- Memberikan kemudahan dan transparansi bagi pelanggan dalam menemukan jasa permak terpercaya
- Mendukung ekosistem ekonomi lokal dan UMKM jasa konveksi/alterasi di Kota Bandung

### 1.5 References

- IEEE Std 830-1998, IEEE Recommended Practice for Software Requirements Specifications
- Dokumen UI/UX Wireframe PermakIn (Figma), April 2025
- Entity Relationship Diagram (ERD) PermakIn, April 2025
- [Laravel 11 Documentation](https://laravel.com/docs/11.x)
- [Flutter Documentation](https://docs.flutter.dev)
- [FastAPI Documentation](https://fastapi.tiangolo.com)
- [Docker & Docker Compose Documentation](https://docs.docker.com)
- [Firebase Cloud Messaging (FCM)](https://firebase.google.com/docs/cloud-messaging)

---

## 2. Overall Description

### 2.1 Product Perspective

PermakIn adalah produk baru yang berdiri sendiri (self-contained), tidak merupakan bagian dari sistem yang lebih besar, dan tidak merupakan pengganti sistem yang sudah ada. Sistem ini merespons kekosongan pasar: belum adanya platform digital khusus yang menghubungkan pelanggan dengan penjahit/tukang permak lokal di Bandung.

Dari perspektif sistem, PermakIn terdiri dari empat komponen utama yang saling terintegrasi: 

1. Aplikasi mobile Flutter sebagai antarmuka pengguna
2. API backend Laravel sebagai pusat logika bisnis
3. Microservice ML (Python/FastAPI) untuk fitur cerdas
4. Automation engine n8n untuk notifikasi

Komponen-komponen ini berkomunikasi melalui REST API dan webhook dalam lingkungan Docker.

### 2.2 Product Functions

Fungsi-fungsi utama yang harus dilakukan oleh sistem PermakIn meliputi:

- **Autentikasi & Manajemen Pengguna:** Registrasi, login, reset password, dan manajemen profil untuk dua peran (pelanggan dan penjahit)
- **Pencarian & Eksplorasi Penjahit:** Pencarian berdasarkan lokasi/jarak, filter rating dan harga, tampil profil/portofolio, serta rekomendasi berbasis ML
- **Pemesanan Jasa Permak:** Pembuatan pesanan dengan foto pakaian, konfirmasi harga & estimasi waktu, tracking status real-time, dan riwayat pesanan
- **Notifikasi & Komunikasi:** Push notification via FCM untuk perubahan status, dan fitur chat in-app antara pelanggan dan penjahit
- **Ulasan & Rating:** Pemberian rating bintang dan ulasan teks, analisis sentimen otomatis menggunakan ML
- **Dashboard Penjahit:** Pengelolaan layanan, portofolio, pesanan masuk, dan laporan ringkasan

### 2.3 User Classes and Characteristics

Sistem PermakIn melayani tiga kelas pengguna utama:

#### Pelanggan (Customer)
Pengguna akhir yang mencari dan memesan layanan permak. Umumnya masyarakat umum dengan kemampuan teknis dasar (terbiasa menggunakan aplikasi mobile). Frekuensi penggunaan bervariasi, dari sekali hingga beberapa kali per bulan.

#### Penjahit/Tukang Permak (Tailor)
Mitra usaha yang mendaftarkan dan mengelola layanan mereka. Kemampuan teknis bervariasi; sistem harus menyediakan antarmuka yang sederhana dan intuitif. Frekuensi penggunaan tinggi (setiap hari untuk cek dan kelola pesanan).

#### Administrator (Admin)
Pengelola sistem yang melakukan verifikasi mitra penjahit dan moderasi konten. Pengguna internal dengan akses penuh ke seluruh data. (Fitur panel admin dikembangkan pada iterasi berikutnya; v1.0 menggunakan auto-approve.)

### 2.4 Operating Environment

Lingkungan operasional sistem PermakIn:

- **Platform Mobile:** Android (minimum API level 21, Android 5.0 Lollipop). Aplikasi dibangun dengan Flutter/Dart
- **Server Backend:** VPS Linux (Ubuntu 22.04 LTS) dengan Docker dan Docker Compose
- **Konektivitas:** Membutuhkan koneksi internet aktif (minimum 3G) untuk semua fitur utama
- **Browser / Admin Panel:** Tidak diperlukan untuk v1.0 (admin panel dikembangkan iterasi berikutnya)
- **OS Server:** Ubuntu 22.04 LTS, Docker Engine 24+, Docker Compose v2

### 2.5 Design and Implementation Constraints

Batasan-batasan yang membatasi pilihan desain pengembang:

- Sistem hanya mencakup layanan alterasi/permak pakaian; tidak termasuk pembuatan pakaian baru dari nol
- Cakupan geografis v1.0 dibatasi pada wilayah Kota Bandung dan sekitarnya
- Pembayaran v1.0 dilakukan secara manual (COD/transfer); tidak terintegrasi payment gateway
- Platform mobile v1.0 hanya untuk Android; iOS pada iterasi berikutnya
- Backend wajib menggunakan Laravel 11 (PHP) sesuai stack yang ditetapkan tim
- Database wajib menggunakan MySQL 8 sesuai stack yang ditetapkan tim
- Seluruh service di-deploy dalam satu Docker Compose stack pada satu VPS
- ML service dikembangkan dengan Python (FastAPI + scikit-learn/transformers)

### 2.6 User Documentation

Komponen dokumentasi pengguna yang akan disertakan:

- **In-App Onboarding:** Panduan singkat (walkthrough) pertama kali buka aplikasi untuk pelanggan dan penjahit
- **FAQ In-App:** Halaman pertanyaan umum yang dapat diakses dari menu pengaturan
- **README Developer:** Dokumentasi instalasi dan konfigurasi sistem untuk tim pengembang (tersedia di repository Git)
- **API Documentation:** Dokumentasi endpoint API menggunakan Swagger/OpenAPI (tersedia di `/api/docs` pada server staging)

### 2.7 Assumptions and Dependencies

#### Asumsi-asumsi

- Pengguna (pelanggan maupun penjahit) memiliki smartphone Android dengan koneksi internet
- Penjahit bersedia mendaftarkan diri dan memperbarui status pesanan secara aktif melalui aplikasi
- Server VPS memiliki spesifikasi minimum: 2 vCPU, 4 GB RAM, 40 GB SSD untuk menjalankan seluruh Docker containers
- Firebase Cloud Messaging (FCM) tersedia dan dapat diakses oleh n8n untuk pengiriman push notification

#### Dependensi Eksternal

- **Google Maps SDK (Flutter):** Digunakan untuk menampilkan peta dan lokasi penjahit. Memerlukan API key aktif
- **Firebase Cloud Messaging:** Untuk push notification. Memerlukan project Firebase aktif
- **n8n (self-hosted):** Untuk automation notifikasi. Berjalan sebagai container terpisah
- **MinIO / S3-compatible storage:** Untuk menyimpan foto pakaian dan portofolio

---

## 3. External Interface Requirements

### 3.1 User Interfaces

Antarmuka pengguna (UI) sistem PermakIn dirancang mengikuti prinsip Material Design 3 dari Google, dengan panduan:

- Navigasi utama menggunakan Bottom Navigation Bar dengan 4 tab: Beranda, Pencarian, Pesanan, Profil
- Komponen UI menggunakan Flutter Widget standar (AppBar, Card, ListTile, FloatingActionButton)
- Palet warna menggunakan tema biru-putih untuk mencerminkan kepercayaan dan profesionalisme
- Tombol aksi utama menggunakan ElevatedButton; aksi sekunder menggunakan TextButton
- Pesan error ditampilkan menggunakan SnackBar atau Dialog dengan teks yang jelas dan actionable
- Semua form memiliki validasi client-side sebelum request dikirim ke API
- Wireframe dan prototype UI/UX tersedia di dokumen Figma terpisah

### 3.2 Hardware Interfaces

Sistem PermakIn berinteraksi dengan hardware berikut pada perangkat pengguna:

- **GPS / Location Services:** Digunakan untuk menentukan lokasi pengguna dan menghitung jarak ke penjahit terdekat. Aplikasi meminta izin `ACCESS_FINE_LOCATION`
- **Kamera:** Digunakan untuk mengambil foto pakaian saat membuat pesanan. Aplikasi meminta izin `CAMERA`
- **Storage/Galeri:** Digunakan untuk memilih foto dari galeri. Aplikasi meminta izin `READ_EXTERNAL_STORAGE`
- **Push Notification (FCM):** Menggunakan Firebase Cloud Messaging yang berinteraksi dengan hardware notification system Android
- **Jaringan (Network Interface):** Aplikasi memerlukan koneksi internet aktif melalui WiFi atau data seluler (minimum 3G)

### 3.3 Software Interfaces

PermakIn berinteraksi dengan komponen software berikut:

- **Google Maps SDK for Flutter:** Menampilkan peta interaktif dengan marker lokasi penjahit. Versi: `google_maps_flutter ^2.x`
- **Firebase Cloud Messaging (FCM):** Untuk pengiriman push notification ke perangkat Android. Terintegrasi via n8n
- **Laravel Sanctum:** Untuk manajemen token autentikasi JWT pada API backend
- **MySQL 8.0:** Database relasional untuk penyimpanan data utama sistem
- **Redis 7:** Untuk caching data dan manajemen queue jobs pada Laravel
- **MinIO (S3-compatible):** Object storage untuk file foto. Diakses via AWS SDK S3-compatible
- **scikit-learn / Hugging Face Transformers:** Library ML Python untuk rekomendasi dan analisis sentimen

### 3.4 Communications Interfaces

Protokol dan mekanisme komunikasi yang digunakan sistem:

- **HTTPS/REST:** Semua komunikasi antara Mobile App dan API Laravel menggunakan HTTPS (TLS 1.2+). Format data: JSON. Autentikasi: Bearer Token JWT di header `Authorization`
- **HTTP Internal Docker Network:** Komunikasi antara API Laravel dan ML Service (FastAPI) menggunakan HTTP biasa pada jaringan internal Docker yang terisolasi (tidak terekspos ke publik)
- **Webhook HTTP:** API Laravel mengirim HTTP POST webhook ke n8n untuk men-trigger workflow notifikasi pada event tertentu (pesanan baru, perubahan status, dll.)
- **FCM Protocol:** n8n menggunakan FCM API untuk mengirim push notification ke perangkat mobile pengguna
- **TCP/IP:** Koneksi antara Laravel container dan MySQL/Redis container menggunakan TCP pada jaringan internal Docker

---

## 4. System Features

### 4.1 Fitur Autentikasi & Manajemen Pengguna

#### 4.1.1 Description and Priority

Fitur ini mencakup seluruh mekanisme autentikasi pengguna termasuk registrasi, login, logout, reset password, dan pembaruan profil. Mendukung dua peran: pelanggan (customer) dan penjahit (tailor). **Prioritas: Tinggi**.

#### 4.1.2 Stimulus/Response Sequences

- **Registrasi:** Pengguna membuka aplikasi dan memilih Daftar → sistem menampilkan form registrasi → pengguna mengisi data → sistem memvalidasi dan membuat akun → pengguna menerima konfirmasi
- **Login:** Pengguna memilih Login → mengisi email & password → sistem memverifikasi via API → sistem mengembalikan JWT token → pengguna diarahkan ke halaman utama
- **Reset Password:** Pengguna memilih Lupa Password → memasukkan email → sistem mengirim OTP → pengguna memasukkan OTP & password baru → sistem mengupdate password

#### 4.1.3 Functional Requirements

| Kode | Nama Fungsi | Deskripsi |
|------|-------------|-----------|
| F-01 | Registrasi Pelanggan | Pengguna dapat mendaftar sebagai pelanggan dengan email, nomor HP, nama, dan kata sandi |
| F-02 | Registrasi Penjahit | Penjahit dapat mendaftar sebagai mitra dengan informasi bisnis, spesialisasi, dan lokasi |
| F-03 | Login/Logout | Pengguna dan penjahit dapat login menggunakan email+password; sistem mendukung token JWT |
| F-04 | Reset Kata Sandi | Pengguna dapat mereset kata sandi via email/OTP |
| F-05 | Manajemen Profil | Pengguna dapat memperbarui profil, foto, dan informasi kontak |

### 4.2 Fitur Pencarian & Eksplorasi Penjahit

#### 4.2.1 Description and Priority

Fitur ini memungkinkan pelanggan menemukan penjahit yang sesuai kebutuhan berdasarkan lokasi, jenis layanan, rating, dan harga. Termasuk rekomendasi berbasis machine learning. **Prioritas: Tinggi**.

#### 4.2.2 Stimulus/Response Sequences

- Pelanggan membuka tab Pencarian → mengaktifkan GPS → sistem menampilkan daftar penjahit terdekat → pelanggan dapat memfilter dan mengurutkan hasil
- Pelanggan memilih penjahit → sistem menampilkan halaman profil lengkap dengan layanan, portofolio, dan ulasan
- Sistem ML secara otomatis menganalisis riwayat pemesanan → memberikan saran penjahit di halaman Beranda

#### 4.2.3 Functional Requirements

| Kode | Nama Fungsi | Deskripsi |
|------|-------------|-----------|
| F-06 | Cari Penjahit | Pelanggan dapat mencari penjahit berdasarkan nama, lokasi/jarak, atau jenis layanan |
| F-07 | Filter & Sorting | Pelanggan dapat memfilter penjahit berdasarkan rating, harga, dan jarak; mengurutkan hasil |
| F-08 | Profil Penjahit | Pelanggan dapat melihat profil lengkap penjahit: foto, alamat, layanan, estimasi harga, portofolio, dan ulasan |
| F-09 | Rekomendasi ML | Sistem memberikan rekomendasi penjahit berbasis riwayat pemesanan dan preferensi pengguna (ML) |

### 4.3 Fitur Pemesanan

#### 4.3.1 Description and Priority

Fitur inti sistem yang mengelola seluruh alur pemesanan jasa permak dari pembuatan pesanan oleh pelanggan hingga penyelesaian oleh penjahit. **Prioritas: Tinggi**.

#### 4.3.2 Stimulus/Response Sequences

- Pelanggan memilih penjahit → memilih layanan → mengisi form pesanan (deskripsi + foto) → mengirim pesanan → sistem menyimpan dengan status PENDING → penjahit menerima notifikasi
- Penjahit membuka notifikasi pesanan → melihat detail → menerima/menolak → jika diterima: mengisi harga dan estimasi waktu → sistem mengupdate status ke CONFIRMED
- Penjahit mengupdate status pengerjaan → sistem mengirim notifikasi ke pelanggan → pelanggan memantau status secara real-time

#### 4.3.3 Functional Requirements

| Kode | Nama Fungsi | Deskripsi |
|------|-------------|-----------|
| F-10 | Buat Pesanan | Pelanggan dapat membuat pesanan dengan deskripsi kebutuhan, foto pakaian, dan pilihan layanan |
| F-11 | Konfirmasi Pesanan | Penjahit menerima notifikasi dan dapat menerima/menolak pesanan dengan estimasi harga & waktu |
| F-12 | Tracking Status | Pelanggan dapat memantau status pesanan secara real-time (Menunggu, Diproses, Selesai, Dibatalkan) |
| F-13 | Batalkan Pesanan | Pelanggan dapat membatalkan pesanan sebelum penjahit mulai mengerjakan |
| F-14 | Riwayat Pesanan | Pelanggan dan penjahit dapat melihat riwayat pesanan beserta statusnya |

### 4.4 Fitur Notifikasi & Komunikasi

#### 4.4.1 Description and Priority

Fitur yang menjaga komunikasi antara pelanggan dan penjahit melalui push notification dan pesan dalam aplikasi. **Prioritas: Sedang**.

#### 4.4.2 Stimulus/Response Sequences

- Terjadi perubahan status pesanan → API Laravel mengirim webhook ke n8n → n8n mengirim push notification via FCM → perangkat pengguna menerima notifikasi
- Pelanggan/penjahit membuka halaman pesanan → memilih tab Chat → mengetik pesan → sistem menyimpan dan menampilkan pesan secara real-time

#### 4.4.3 Functional Requirements

| Kode | Nama Fungsi | Deskripsi |
|------|-------------|-----------|
| F-15 | Push Notification | Sistem mengirim push notification untuk perubahan status pesanan (via n8n workflow automation) |
| F-16 | Chat In-App | Pelanggan dan penjahit dapat berkomunikasi via fitur pesan dalam aplikasi |

### 4.5 Fitur Ulasan & Rating

#### 4.5.1 Description and Priority

Fitur yang memungkinkan pelanggan memberikan umpan balik atas layanan penjahit, disertai analisis sentimen otomatis untuk membantu sistem memahami kualitas layanan. **Prioritas: Sedang**.

#### 4.5.2 Stimulus/Response Sequences

- Pesanan berstatus DONE → pelanggan mendapat prompt untuk memberikan ulasan → mengisi rating dan komentar → submit → sistem menyimpan ulasan dan memproses sentimen via ML service
- Calon pelanggan membuka profil penjahit → melihat daftar ulasan dengan skor rata-rata → membuat keputusan pemilihan penjahit

#### 4.5.3 Functional Requirements

| Kode | Nama Fungsi | Deskripsi |
|------|-------------|-----------|
| F-17 | Beri Ulasan | Pelanggan dapat memberikan rating (1-5 bintang) dan ulasan teks setelah pesanan selesai |
| F-18 | Lihat Ulasan | Pengguna dapat melihat semua ulasan penjahit pada halaman profil penjahit |
| F-19 | Analisis Sentimen | Sistem menganalisis sentimen ulasan teks secara otomatis menggunakan model ML |

### 4.6 Fitur Dashboard Penjahit

#### 4.6.1 Description and Priority

Dashboard khusus untuk penjahit dalam mengelola seluruh aspek usaha mereka pada platform PermakIn, mulai dari pengelolaan layanan, portofolio, pesanan masuk, hingga laporan ringkasan. **Prioritas: Tinggi**.

#### 4.6.2 Stimulus/Response Sequences

- Penjahit login → mengakses tab Dashboard → melihat ringkasan pesanan aktif, rating, dan pendapatan
- Penjahit memilih Kelola Layanan → menambah/mengubah/menghapus jenis layanan beserta harga
- Penjahit memilih Portofolio → mengunggah foto hasil pekerjaan → foto tampil di halaman profil publik

#### 4.6.3 Functional Requirements

| Kode | Nama Fungsi | Deskripsi |
|------|-------------|-----------|
| F-20 | Kelola Layanan | Penjahit dapat menambah, mengubah, dan menghapus daftar layanan beserta harga estimasi |
| F-21 | Kelola Portofolio | Penjahit dapat mengunggah foto hasil pekerjaan sebagai portofolio |
| F-22 | Manajemen Pesanan | Penjahit dapat melihat, menerima, menolak, dan memperbarui status pesanan masuk |
| F-23 | Laporan Sederhana | Penjahit dapat melihat ringkasan jumlah pesanan dan pendapatan per periode |

---

## 5. Other Nonfunctional Requirements

### 5.1 Performance Requirements

Kebutuhan performa sistem PermakIn:

- Response time API untuk endpoint utama (pencarian, pesanan) tidak lebih dari 2 detik pada kondisi jaringan normal
- Sistem mampu menangani minimal 100 concurrent users tanpa degradasi performa yang signifikan
- Upload foto pakaian (maks 5MB) harus selesai dalam 10 detik pada koneksi 3G
- Push notification terkirim dalam 30 detik setelah event terjadi
- Query pencarian penjahit terdekat (berbasis GPS) mengembalikan hasil dalam 3 detik
- Waktu loading halaman profil penjahit (termasuk foto) tidak lebih dari 4 detik

### 5.2 Safety Requirements

Kebutuhan keamanan data dan keselamatan pengguna:

- Sistem tidak menyimpan data kartu kredit atau informasi pembayaran sensitif (pembayaran dilakukan manual di luar sistem)
- Foto pakaian yang diunggah hanya dapat diakses oleh pelanggan pemesan dan penjahit terkait
- Sistem harus memiliki mekanisme backup database minimal sekali sehari untuk mencegah kehilangan data
- Jika ML service tidak tersedia, sistem tetap berfungsi normal tanpa fitur rekomendasi dan sentimen (graceful degradation)

### 5.3 Security Requirements

Kebutuhan keamanan sistem:

- Seluruh komunikasi antara client dan server menggunakan HTTPS (TLS 1.2 atau lebih tinggi)
- Password pengguna disimpan dalam bentuk hash menggunakan bcrypt (cost factor minimum 10)
- Autentikasi API menggunakan JWT (JSON Web Token) dengan masa berlaku 24 jam
- Endpoint yang memerlukan autentikasi dilindungi oleh middleware JWT; akses tanpa token valid mengembalikan HTTP 401
- Validasi input dilakukan di sisi server untuk mencegah SQL Injection dan XSS
- File upload divalidasi tipe (hanya jpg/png) dan ukuran (maks 5MB); file disimpan di object storage terpisah dari code base
- Hanya port 80 dan 443 yang diekspos ke publik; semua port internal Docker tidak dapat diakses dari luar

### 5.4 Software Quality Attributes

Atribut kualitas software yang harus dipenuhi:

| Atribut | Deskripsi |
|---------|-----------|
| **Availability** | Sistem tersedia minimal 99% uptime (downtime tidak lebih dari 7 jam/bulan) |
| **Maintainability** | Kode mengikuti standar PSR-12 (Laravel) dan Dart Style Guide (Flutter). Setiap modul memiliki unit test minimal 70% coverage |
| **Scalability** | Arsitektur Docker memungkinkan horizontal scaling pada API dan ML service di masa mendatang |
| **Portability** | Aplikasi mobile berjalan pada semua perangkat Android dengan API level >= 21 |
| **Usability** | Alur pemesanan utama dapat diselesaikan dalam tidak lebih dari 5 langkah/screen |
| **Testability** | API backend memiliki collection Postman yang didokumentasikan dan dapat dijalankan untuk testing |

### 5.5 Business Rules

Aturan bisnis yang harus diimplementasikan dalam sistem:

1. Seorang pengguna hanya dapat memiliki satu peran: pelanggan ATAU penjahit. Perubahan peran tidak diizinkan setelah registrasi
2. Pelanggan hanya dapat memberikan ulasan pada pesanan yang berstatus DONE
3. Pelanggan hanya dapat membatalkan pesanan yang masih berstatus PENDING (belum dikonfirmasi penjahit)
4. Penjahit hanya dapat mengelola (ubah/hapus) layanan dan pesanan milik mereka sendiri
5. Rating penjahit dihitung sebagai rata-rata dari semua ulasan yang diterima (diupdate otomatis saat ulasan baru masuk)
6. Pesanan yang sudah CONFIRMED tidak dapat dibatalkan secara sepihak oleh pelanggan tanpa konfirmasi penjahit

---

## 6. System Architecture & API Design

### 6.1 Component Diagram

Sistem PermakIn terdiri dari empat komponen utama yang terpisah dan berkomunikasi melalui REST API dan webhook:

| Komponen | Teknologi | Peran & Tanggung Jawab |
|----------|-----------|----------------------|
| **Mobile App** | Flutter (Dart) | Antarmuka pengguna untuk pelanggan dan penjahit. Mengonsumsi REST API Laravel. Menampilkan peta, status pesanan, notifikasi push. |
| **API Backend** | Laravel 11 (PHP) | Menyediakan seluruh endpoint RESTful API. Mengelola autentikasi (JWT), logika bisnis, akses database, dan integrasi storage. |
| **ML Service** | Python (FastAPI + scikit-learn/transformers) | Microservice untuk: (1) rekomendasi penjahit berbasis collaborative filtering, (2) analisis sentimen ulasan. |
| **Automation** | n8n (self-hosted) | Workflow automation untuk pengiriman push notification (FCM), notifikasi status, dan reminder ke pengguna. |
| **Database** | MySQL 8 | Menyimpan seluruh data relasional: users, orders, services, reviews, messages, notifications. |
| **Object Storage** | MinIO (Docker) / S3-compatible | Menyimpan foto pakaian dan foto portofolio penjahit. |

#### Alur Komunikasi Antar Komponen

- **Mobile App ↔ API Laravel:** HTTPS REST API (JSON), header `Authorization: Bearer {JWT}`
- **API Laravel → ML Service:** HTTP internal Docker network, POST request dengan payload teks/data
- **API Laravel → n8n:** Webhook HTTP POST saat terjadi event (pesanan baru, perubahan status)
- **n8n → FCM:** Pengiriman push notification ke device pengguna
- **API Laravel ↔ MySQL & Redis:** Koneksi TCP internal Docker via Eloquent ORM dan Redis client
- **API Laravel ↔ MinIO:** S3-compatible SDK untuk upload/download file

### 6.2 Deployment Diagram

Infrastruktur deployment PermakIn menggunakan Docker dan Docker Compose pada satu VPS Linux (Ubuntu 22.04):

| Container / Service | Port | Deskripsi |
|---------------------|------|-----------|
| **nginx** | 80, 443 | Reverse proxy dan SSL termination. Meneruskan request ke container Laravel |
| **app (Laravel PHP-FPM)** | 9000 | Container aplikasi Laravel berjalan dengan PHP-FPM. Berisi source code API |
| **db (MySQL 8)** | 3306 | Container database MySQL. Data persisten menggunakan Docker Volume |
| **ml_service (FastAPI)** | 8001 | Container Python untuk layanan ML (rekomendasi + sentimen) |
| **n8n** | 5678 | Container n8n untuk workflow automation dan notifikasi |
| **minio** | 9000, 9001 | Container object storage untuk menyimpan file/foto |
| **redis** | 6379 | Container Redis untuk caching dan queue jobs Laravel |

#### Topologi Infrastruktur Server

- Seluruh container dijalankan dalam satu Docker Compose stack pada sebuah VPS Linux (Ubuntu 22.04)
- Jaringan internal Docker (bridge network) digunakan untuk komunikasi antar container tanpa expose ke publik
- Hanya port 80 dan 443 (Nginx) yang di-expose ke publik; port lain hanya accessible secara internal
- Data MySQL dan MinIO disimpan pada Docker Named Volume untuk persistensi data
- SSL Certificate dikelola menggunakan Let's Encrypt (Certbot) pada container Nginx

### 6.3 UML Models

#### 6.3.1 Class Diagram

Kelas-kelas domain bisnis utama dalam sistem PermakIn:

| Kelas | Atribut Utama | Metode / Operasi |
|-------|--------------|------------------|
| **User** | id, name, email, phone, password, role, created_at | `register()`, `login()`, `updateProfile()`, `logout()` |
| **Customer** | id, user_id, address, location (lat/lng) | `searchTailor()`, `createOrder()`, `rateService()` |
| **Tailor** | id, user_id, shop_name, description, address, location, is_verified, rating_avg | `addService()`, `updateService()`, `acceptOrder()`, `rejectOrder()`, `updateOrderStatus()` |
| **Service** | id, tailor_id, name, description, min_price, max_price, category | `getByTailor()`, `activate()`, `deactivate()` |
| **Order** | id, customer_id, tailor_id, service_id, description, photo_url, status, agreed_price, deadline, created_at | `create()`, `confirm()`, `cancel()`, `updateStatus()`, `complete()` |
| **OrderStatus** | PENDING, CONFIRMED, IN_PROGRESS, DONE, CANCELLED | (Enum / Lookup table) |
| **Review** | id, order_id, customer_id, tailor_id, rating, comment, sentiment_score, created_at | `create()`, `getSentiment()` |
| **Notification** | id, user_id, type, title, body, is_read, created_at | `send()`, `markRead()` |
| **Message** | id, order_id, sender_id, receiver_id, content, created_at | `send()`, `getHistory()` |
| **Portfolio** | id, tailor_id, image_url, description, created_at | `upload()`, `delete()` |

##### Relasi Antar Kelas

- User memiliki satu Customer atau satu Tailor (1:1 polimorfik berdasarkan role)
- Tailor memiliki banyak Service (1:N) dan banyak Portfolio (1:N)
- Customer membuat banyak Order (1:N); Tailor menerima banyak Order (1:N)
- Order memiliki satu Review (1:1) dan banyak Message (1:N)
- User memiliki banyak Notification (1:N)

#### 6.3.2 Sequence Diagram – Alur Login

| # | Aktor | Mobile App | API Laravel | Database | Keterangan |
|---|-------|-----------|-------------|----------|-----------|
| 1 | Input email & password | Validasi format input | | | Client-side validation |
| 2 | | POST /api/auth/login | Terima request | | HTTP Request |
| 3 | | | Query user by email | Return user data | DB Query |
| 4 | | | Hash & verifikasi password | | bcrypt verify |
| 5 | | | Generate JWT Token | | Sanctum / JWT |
| 6 | | Terima token + user data | Return 200 OK + token | | HTTP Response |
| 7 | Diarahkan ke halaman utama | Simpan token ke storage | | | Local storage |

#### 6.3.3 Sequence Diagram – Alur Pembuatan Pesanan

| # | Pelanggan | Mobile App | API Laravel | Database | Keterangan |
|---|-----------|-----------|-------------|----------|-----------|
| 1 | Pilih penjahit & layanan | Tampil form pesanan | | | UI Form |
| 2 | Isi deskripsi & upload foto | Compress & prepare payload | | | Client processing |
| 3 | | POST /api/orders (+JWT) | Validasi token & payload | | Auth middleware |
| 4 | | | Upload foto ke storage | | Laravel Storage/S3 |
| 5 | | | Simpan order (PENDING) | Insert orders | DB Write |
| 6 | | | Trigger notifikasi via n8n | | Webhook n8n |
| 7 | | Tampil halaman status | Return 201 + order data | | HTTP Response |
| 8 | | | | | UI Update |
| 9 | Penjahit terima notifikasi | Penjahit: konfirmasi/tolak | PATCH /api/orders/{id}/confirm | Update order status | Penjahit response |

### 6.4 API Contract Design

**Base URL:** `https://api.permakin.id/api/v1`  
**Format:** `application/json`  
**Autentikasi:** Bearer Token (JWT)

#### 6.4.1 Ringkasan Endpoint

| Method | Endpoint | Auth | Status | Deskripsi |
|--------|----------|------|--------|-----------|
| POST | /auth/register | Public | 201, 422 | Registrasi pengguna |
| POST | /auth/login | Public | 200, 401 | Login & dapatkan token |
| POST | /auth/logout | Bearer | 200 | Logout |
| GET | /tailors | Public | 200 | Daftar penjahit + filter |
| GET | /tailors/{id} | Public | 200, 404 | Detail penjahit |
| PUT | /tailors/{id} | Bearer (tailor) | 200, 403 | Update profil penjahit |
| GET | /tailors/{id}/services | Public | 200 | Daftar layanan penjahit |
| POST | /tailors/{id}/services | Bearer (tailor) | 201, 422 | Tambah layanan |
| DELETE | /tailors/{id}/services/{sid} | Bearer (tailor) | 204, 403 | Hapus layanan |
| POST | /orders | Bearer (customer) | 201, 422 | Buat pesanan baru |
| GET | /orders | Bearer | 200 | Daftar pesanan pengguna |
| GET | /orders/{id} | Bearer | 200, 404 | Detail pesanan |
| PATCH | /orders/{id}/confirm | Bearer (tailor) | 200, 403 | Konfirmasi pesanan |
| PATCH | /orders/{id}/status | Bearer (tailor) | 200, 403 | Update status pesanan |
| PATCH | /orders/{id}/cancel | Bearer (customer) | 200, 403 | Batalkan pesanan |
| POST | /orders/{id}/review | Bearer (customer) | 201, 422 | Beri ulasan pesanan |
| GET | /tailors/{id}/reviews | Public | 200 | Daftar ulasan penjahit |
| POST | /ml/recommend | Internal only | 200 | Rekomendasi penjahit (ML) |
| POST | /ml/sentiment | Internal only | 200 | Analisis sentimen (ML) |

#### 6.4.2 POST /auth/register

**Deskripsi:** Registrasi pengguna baru (pelanggan atau penjahit).

| Field | Tipe | Required | Keterangan |
|-------|------|----------|-----------|
| name | string | Ya | Nama lengkap pengguna |
| email | string | Ya | Email valid, unik dalam sistem |
| phone | string | Ya | Nomor HP (format 08xx) |
| password | string | Ya | Min. 8 karakter |
| role | enum | Ya | `customer` \| `tailor` |

**Response 201 Created:**
```json
{
  "status": "success",
  "data": {
    "user": { /* ... */ },
    "token": "eyJ..."
  }
}
```

**Response 422 Unprocessable Entity:** Validasi gagal (field duplikat atau format salah).

#### 6.4.3 POST /auth/login

**Deskripsi:** Login pengguna dan mendapatkan JWT token.

| Field | Tipe | Required | Keterangan |
|-------|------|----------|-----------|
| email | string | Ya | Email terdaftar |
| password | string | Ya | Kata sandi pengguna |

**Response 200 OK:**
```json
{
  "status": "success",
  "data": {
    "token": "eyJ...",
    "expires_in": 3600,
    "user": { /* ... */ }
  }
}
```

**Response 401 Unauthorized:** Kredensial tidak valid.

#### 6.4.4 POST /orders

**Deskripsi:** Buat pesanan baru. **Auth:** Bearer (customer).

| Field | Tipe | Required | Keterangan |
|-------|------|----------|-----------|
| tailor_id | integer | Ya | ID penjahit yang dipilih |
| service_id | integer | Ya | ID layanan yang dipilih |
| description | string | Ya | Deskripsi kebutuhan permak |
| photo | file (multipart) | Tidak | Foto pakaian (max 5MB, jpg/png) |
| pickup_method | enum | Ya | `self_deliver` \| `request_pickup` |

**Response 201 Created:** Data pesanan baru dengan status PENDING.

#### 6.4.5 ML Service Endpoints (Internal)

**POST /ml/recommend**
- **Body:** `{ customer_id, lat, lng, limit }`
- **Response:** `{ tailors: [{tailor_id, score}] }`

**POST /ml/sentiment**
- **Body:** `{ text }`
- **Response:** `{ label: "positive"|"negative"|"neutral", score: 0.96 }`

**Catatan:** Endpoint ML hanya dapat diakses dari container API Laravel melalui jaringan internal Docker.

---

## 7. Other Requirements

Kebutuhan tambahan yang tidak masuk dalam kategori di atas:

- **Lokalisasi:** Antarmuka aplikasi menggunakan Bahasa Indonesia sebagai bahasa utama
- **Aksesibilitas:** Teks minimal ukuran 14sp; kontras warna mengikuti standar WCAG 2.1 Level AA
- **Audit Log:** Setiap perubahan status pesanan dicatat dalam log dengan timestamp dan user yang melakukan perubahan
- **Data Retention:** Data pesanan disimpan minimal 1 tahun; data pengguna yang dihapus dianonimkan (bukan dihapus permanen)
- **Compliance:** Sistem mengikuti ketentuan perlindungan data pribadi sesuai regulasi yang berlaku di Indonesia

---

## Appendix A: Glossary

| Istilah | Definisi |
|---------|----------|
| **Permak / Alterasi** | Proses penyesuaian/modifikasi pakaian yang sudah jadi agar sesuai ukuran atau selera pelanggan |
| **Penjahit / Tailor** | Mitra usaha yang terdaftar di PermakIn dan menyediakan layanan permak/alterasi pakaian |
| **Pelanggan / Customer** | Pengguna akhir yang memesan layanan permak melalui aplikasi PermakIn |
| **JWT** | JSON Web Token – standar terbuka untuk autentikasi stateless berbasis token |
| **REST API** | Representational State Transfer API – gaya arsitektur untuk layanan web berbasis HTTP |
| **FCM** | Firebase Cloud Messaging – layanan Google untuk pengiriman push notification ke perangkat mobile |
| **n8n** | Platform workflow automation open-source yang digunakan untuk mengotomasi pengiriman notifikasi |
| **Flutter** | Framework UI open-source dari Google untuk membangun aplikasi mobile lintas platform |
| **Docker** | Platform containerization untuk mengemas dan menjalankan aplikasi dalam lingkungan terisolasi |
| **ML Service** | Microservice berbasis Python yang menyediakan fitur machine learning (rekomendasi & sentimen) |
| **MinIO** | Object storage open-source yang kompatibel dengan Amazon S3, digunakan untuk menyimpan file foto |
| **COD** | Cash on Delivery – metode pembayaran tunai saat penerimaan barang/layanan |

---

## Appendix B: Analysis Models

Model analisis tambahan yang tersedia sebagai dokumen pendukung terpisah:

- **Entity Relationship Diagram (ERD)** – Menggambarkan struktur database relasional PermakIn dengan seluruh tabel dan relasi foreign key
- **UI/UX Wireframe & Prototype (Figma)** – Wireframe dan interactive prototype seluruh layar aplikasi mobile PermakIn
- **Data Flow Diagram (DFD) Level 0 & Level 1** – Diagram aliran data sistem PermakIn dari perspektif pelanggan dan penjahit
- **Business Process Model (BPMN)** – Diagram alur proses bisnis pemesanan dan pendaftaran penjahit

---

## Appendix C: To Be Determined (TBD) List

| ID | Item TBD | Target Resolusi |
|----|----------|-----------------|
| TBD-01 | Integrasi payment gateway (Midtrans/Xendit) untuk pembayaran digital | Iterasi v1.1 (pasca UTS) |
| TBD-02 | Pengembangan aplikasi iOS (Flutter multi-platform build) | Iterasi v2.0 |
| TBD-03 | Panel administrasi web untuk verifikasi mitra penjahit | Iterasi v1.1 |
| TBD-04 | Fitur penjemputan/pickup menggunakan ojek online pihak ketiga | Iterasi v1.2 |
| TBD-05 | Skema harga dan monetisasi platform (komisi/subscription) | Sebelum launch produksi |
| TBD-06 | Training dataset untuk model ML rekomendasi (collaborative filtering) | Parallel dengan pengembangan backend |

---

## Penutup

Dokumen Software Requirements Specification (SRS) ini merupakan acuan utama dalam pengembangan sistem PermakIn – Platform Pemesanan Jasa Permak Baju Lokal Berbasis Mobile. Dokumen ini disusun mengikuti standar IEEE 830-1998 dan akan terus diperbarui seiring perkembangan proyek; setiap perubahan dicatat pada tabel Revision History.

Pada tahap evaluasi UTS ini, dokumen mencakup:
- Deskripsi proyek secara menyeluruh
- Kebutuhan fungsional (23 fungsi dalam 6 modul)
- External interface requirements
- System features lengkap dengan stimulus/response sequences
- Kebutuhan non-fungsional
- Perancangan arsitektur (Component & Deployment Diagram)
- Pemodelan UML (Class Diagram & Sequence Diagram)
- Desain kontrak API lengkap (19 endpoint)

---

**Tim Pengembang** – Mata Kuliah Rekayasa Sistem Informasi Kelas A2  
Program Studi Sistem Informasi | Bandung, 2025
