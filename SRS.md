# Software Requirements Specification (SRS): Aplikasi Pemesanan Lapangan Olahraga
**Versi:** 1.0  
**Tanggal:** 17 September 2025  
**Author:**
- Ahmad Rois (221240001239)
- M. Gilang M.W. Sabdokafi (221240001248)

**Status:** Draft  
---

## 1. Pendahuluan

### 1.1 Tujuan
Dokumen ini bertujuan untuk mendefinisikan spesifikasi fungsional dan non-fungsional dari **"Aplikasi Pemesanan Lapangan Olahraga"**. Dokumen ini menjadi acuan bagi tim pengembang dalam desain, implementasi, dan pengujian aplikasi. Fokus utama adalah menyediakan sistem pemesanan lapangan yang sederhana, ringan, dan mudah digunakan.

### 1.2 Ruang Lingkup Produk
Aplikasi ini adalah aplikasi mobile berbasis Flutter yang memungkinkan pengguna untuk:
- Melihat daftar lapangan (Futsal, Badminton, Voli).
- Membuat pemesanan lapangan berdasarkan tanggal dan jam.
- Mengelola status pemesanan (Aktif, Batal, Selesai).
- Melihat riwayat pemesanan.

Fitur-fitur lanjutan seperti pembayaran online, notifikasi, atau integrasi eksternal dapat ditambahkan di rilis berikutnya.

### 1.3 Definisi, Akronim, dan Singkatan
- **SRS**: Software Requirements Specification  
- **CRUD**: Create, Read, Update, Delete  
- **UI**: User Interface  
- **MVP**: Minimum Viable Product  
- **DB**: Database  
- **API**: Application Programming Interface  

### 1.4 Referensi
- Brainstorming Aplikasi Pemesanan Lapangan (Sept 2025)  
- Draft ERD Pemesanan Lapangan v1.0  
- Desain Arsitektur Clean Code (Flutter + Riverpod Generator + Drift)

---

## 2. Deskripsi Umum

### 2.1 Perspektif Produk
Aplikasi ini adalah aplikasi mobile mandiri (standalone) yang berjalan di Android & iOS. Data inti disimpan di database lokal menggunakan Drift. Tidak ada ketergantungan ke API eksternal pada versi awal (MVP).

### 2.2 Fungsi Produk
- **Fungsi Inti**: Manajemen lapangan, manajemen pemesanan, riwayat booking.  
- **Fungsi Utilitas**: Validasi jadwal, pencatatan pembatalan/penyelesaian.  
- **Fungsi Logging**: Logging event (booking baru, pembatalan, selesai).  

### 2.3 Karakteristik Pengguna
- Pengguna umum (pemain) → ingin melakukan booking lapangan dengan cepat.  
- Pengelola lapangan → mencatat pemesanan, mengubah status booking, dan melihat riwayat.  
- Pengguna memiliki kemampuan dasar menggunakan smartphone.  

### 2.4 Batasan (Constraints)
- **C-1**: Aplikasi dibangun dengan Flutter.  
- **C-2**: State management menggunakan Riverpod + Riverpod Generator.  
- **C-3**: Model data menggunakan Freezed.  
- **C-4**: Database lokal menggunakan Drift.  
- **C-5**: Navigasi menggunakan Go_Router.  
- **C-6**: Error handling menggunakan fpdart.  
- **C-7**: Logging menggunakan logger.  
- **C-8**: Aplikasi mendukung Android API level 21+ dan iOS 12+.  

---

## 3. Persyaratan Spesifik

### 3.1 Persyaratan Fungsional

#### Modul 1: Manajemen Lapangan
- **REQ-FUNC-001**: Sistem harus menampilkan daftar lapangan (Futsal, Badminton, Voli).  
- **REQ-FUNC-002**: Pengelola dapat menambah lapangan baru (opsional).  
- **REQ-FUNC-003**: Pengelola dapat mengubah atau menghapus lapangan.  

#### Modul 2: Pemesanan
- **REQ-FUNC-004**: Sistem harus menyediakan form untuk membuat pemesanan dengan input: Nama Pemesan, Lapangan, Tanggal, Jam Mulai, Jam Selesai.  
- **REQ-FUNC-005**: Sistem harus memvalidasi slot waktu agar tidak bentrok dengan pemesanan lain.  
- **REQ-FUNC-006**: Sistem harus menyimpan pemesanan ke database lokal.  

#### Modul 3: Kelola Pemesanan
- **REQ-FUNC-007**: Sistem harus menampilkan daftar pemesanan aktif.  
- **REQ-FUNC-008**: Pengguna dapat mengubah status pemesanan → `Aktif`, `Batal`, `Selesai`.  
- **REQ-FUNC-009**: Pengguna dapat menghapus pemesanan.  

#### Modul 4: Riwayat
- **REQ-FUNC-010**: Sistem harus menampilkan daftar pemesanan yang sudah selesai/batal.  
- **REQ-FUNC-011**: Sistem harus menyediakan filter riwayat berdasarkan lapangan/tanggal.  

#### Modul 5: Pencarian & Filter (opsional MVP)
- **REQ-FUNC-012**: Sistem harus menyediakan pencarian pemesanan berdasarkan nama pemesan.  
- **REQ-FUNC-013**: Sistem harus menyediakan filter berdasarkan lapangan dan tanggal.  

---

### 3.2 Persyaratan Non-Fungsional
- **REQ-NONF-001 (Kinerja)**:  
  - Waktu cold start aplikasi < 3 detik.  
  - Waktu query jadwal booking < 300 ms pada perangkat kelas menengah.  
- **REQ-NONF-002 (Keandalan)**:  
  - Tingkat crash-free > 99%.  
  - Validasi slot waktu harus konsisten untuk mencegah bentrok.  
- **REQ-NONF-003 (Usability)**:  
  - UI harus simpel dan mengikuti Material Design.  
  - Proses booking maksimal 3 langkah (pilih lapangan → pilih jadwal → simpan).  

---

### 3.3 Persyaratan Antarmuka Eksternal
- **REQ-EXT-001 (Antarmuka Pengguna)**:  
  - UI harus mendukung berbagai ukuran layar.  
  - Navigasi antar halaman menggunakan Go_Router.  
- **REQ-EXT-002 (Antarmuka Perangkat Keras)**:  
  - Aplikasi harus dapat mengakses penyimpanan lokal untuk database Drift.  
- **REQ-EXT-003 (Antarmuka Perangkat Lunak)**:  
  - Tidak ada API eksternal pada versi awal.  
  - Logger digunakan untuk pencatatan internal.  

---
