# Product Requirements Document (PRD): Aplikasi Pemesanan Lapangan Olahraga
**Versi:** 1.0  
**Tanggal:** 17 September 2025  
**Author:**
- Ahmad Rois (221240001239)
- M. Gilang M.W. Sabdokafi (221240001248)

**Status:** Draft  
---

## 1. Latar Belakang & Visi Produk

### 1.1. Masalah yang Diselesaikan
Proses pemesanan lapangan olahraga (futsal, badminton, voli) saat ini masih banyak dilakukan secara manual: catatan buku, telepon, atau chat. Cara ini rawan terjadi kesalahan jadwal, bentrok pemakaian, dan sulit untuk dilacak. Pengelola maupun pemain membutuhkan sistem yang sederhana dan cepat untuk mencatat pemesanan, pembatalan, maupun riwayat penggunaan lapangan.

### 1.2. Visi Produk
Menjadi aplikasi **pemesanan lapangan olahraga** yang **paling ringan, cepat, dan mudah dipakai**, sehingga baik pengelola maupun pemain bisa mengatur jadwal main tanpa ribet.

### 1.3. Proposisi Nilai Utama (Unique Value Proposition)
"Pesan lapangan futsal, badminton, atau voli hanya dengan beberapa tap. Simpel, tanpa ribet, selalu siap dipakai."

---

## 2. Target Pengguna

### Persona 1: Dani, Mahasiswa
- **Kebutuhan:** Booking lapangan futsal untuk main bareng temen-temen kampus.  
- **Frustrasi:** Sering bentrok jadwal karena lapangan dicatat manual.  
- **Tujuan:** Bisa booking cepat, langsung tau lapangan kosong atau penuh.  

### Persona 2: Rina, Pengelola Lapangan
- **Kebutuhan:** Mencatat semua pemesanan, pembatalan, dan riwayat penggunaan.  
- **Frustrasi:** Buku catatan sering hilang, WhatsApp numpuk.  
- **Tujuan:** Bisa mengelola pemesanan lebih rapi dan cepat di satu aplikasi.  

---

## 3. Tujuan & Metrik Kesuksesan

### 3.1. Tujuan Produk
- **Bagi Pengguna:** Memudahkan pemain memesan lapangan olahraga dengan cepat dan tanpa kebingungan.  
- **Bagi Pengelola:** Memberikan sistem catatan digital yang praktis, bebas error, dan bisa dipantau kapan saja.  

### 3.2. Metrik Kesuksesan
- Engagement: Rata-rata booking per pengguna aktif ≥ 2 kali per minggu.  
- Retensi: Day-7 Retention ≥ 25%.  
- Kepuasan: Rating aplikasi ≥ 4.5 di Play Store.  
- Efisiensi: 0% double-booking (jadwal bentrok).  

---

## 4. Fitur & Ruang Lingkup (Scope)

### 4.1. Rilis MVP (Minimum Viable Product)
Fokus ke fungsi inti pemesanan lapangan.

| ID   | Fitur                  | Deskripsi                                                                 | Prioritas |
|------|-------------------------|---------------------------------------------------------------------------|-----------|
| F1.1 | Manajemen Lapangan      | Sistem mendukung 3 lapangan default: Futsal, Badminton, Voli.             | Wajib     |
| F2.1 | Tambah Booking          | User pilih lapangan, tanggal, jam mulai, jam selesai, nama pemesan.       | Wajib     |
| F2.2 | Validasi Jadwal         | Sistem menolak booking jika jam bentrok dengan booking lain.              | Wajib     |
| F3.1 | Kelola Booking          | Update status booking menjadi **Aktif**, **Batal**, atau **Selesai**.    | Wajib     |
| F3.2 | Riwayat Booking         | Lihat daftar booking yang sudah selesai atau dibatalkan.                  | Wajib     |

### 4.2. Rilis Selanjutnya (Post-MVP)
Fitur lanjutan setelah MVP stabil.

| ID   | Fitur                    | Deskripsi                                                                 | Prioritas |
|------|---------------------------|---------------------------------------------------------------------------|-----------|
| F4.1 | Multi-Lapangan per Jenis  | Satu jenis olahraga bisa punya beberapa lapangan (contoh: Futsal 1 & 2). | Tinggi    |
| F5.1 | Pencarian & Filter        | Cari booking berdasarkan nama pemesan, lapangan, atau tanggal.            | Tinggi    |
| F6.1 | Notifikasi Reminder       | Notifikasi X jam sebelum jadwal main.                                     | Medium    |
| F7.1 | Integrasi Pembayaran      | Tambah pembayaran via QRIS / e-wallet.                                    | Medium    |
| F8.1 | Backup & Sync Cloud       | Sinkronisasi data ke server (Appwrite/Firebase).                          | Medium    |
| F9.1 | Multi-Role (Admin/User)   | Pisahkan role pengelola & pemain.                                         | Medium    |

### 4.3. Tidak Termasuk dalam Ruang Lingkup (Out of Scope)
- Fitur membership atau paket langganan.  
- Sistem penilaian/review lapangan.  
- Live chat antara user & admin.  

---

## 5. Alur Pengguna (User Flow)

### 5.1. Alur Utama: Membuat Booking Baru
1. User membuka aplikasi → langsung melihat daftar booking hari ini.  
2. User menekan tombol **+ Booking**.  
3. User memilih lapangan (futsal/badminton/voli), tanggal, jam mulai, jam selesai, dan nama pemesan.  
4. User menekan **Simpan**.  
5. Sistem memvalidasi ketersediaan slot.  
6. Jika tersedia → booking tersimpan dengan status **Aktif**.  

### 5.2. Alur Sekunder: Membatalkan Booking
1. User membuka daftar booking.  
2. User memilih salah satu booking.  
3. User menekan tombol **Batal**.  
4. Sistem mengubah status menjadi **Batal**.  

---

## 6. Persyaratan Non-Fungsional

- **Kinerja:**  
  - Cold start < 3 detik.  
  - Navigasi antar halaman < 300ms.  
  - Animasi & scroll stabil di 60 FPS.  

- **Keamanan:**  
  - Data disimpan lokal menggunakan Drift (sandboxed).  
  - Backup ke cloud (opsional post-MVP).  

- **Usability:**  
  - UI bersih & konsisten.  
  - Booking bisa dibuat dalam ≤ 4 langkah.  

- **Teknologi:**  
  - Flutter sebagai framework utama.  
  - Clean Architecture (domain, data, presentation).  
  - State management: Riverpod Generator.  
  - Data class: Freezed.  
  - Routing: GoRouter.  
  - Error handling: fpdart (Either).  
  - Logging: Logger.  
  - Database lokal: Drift.  

---
