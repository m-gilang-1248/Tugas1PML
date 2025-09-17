# Timeline Pengembangan MVP: Aplikasi Pemesanan Lapangan Olahraga

## Tujuan MVP: Pengguna dapat melakukan reservasi lapangan (futsal, badminton, voli), melihat jadwal, membatalkan pemesanan, dan menandai status selesai.

Sprint 1: Fondasi & Fungsionalitas Inti (Durasi: 2 Minggu)

Tujuan Sprint: Setup proyek, arsitektur clean code, database untuk pemesanan & lapangan, serta fitur inti CRUD booking. Di akhir sprint, aplikasi sudah bisa melakukan booking & menampilkan jadwal meskipun UI masih sederhana.

| Hari | Task ID | Tugas                                     | Keterangan & Dependencies                                                                                                                    | Status                    |   |
| ---- | ------- | ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- | - |
| 1-2  | S1-T01  | Setup Proyek & Konfigurasi Awal           | Buat proyek Flutter baru, install dependensi (Riverpod, Drift, GoRouter, Freezed, fpdart, Logger), setup linter, struktur folder sesuai SDD. | ☐                         |   |
|      | S1-T02  | Desain & Implementasi Database (Drift)    | Buat tabel: **Lapangan**, **Pemesanan**. Tambahkan kolom `id`, `nama_lapangan`, `waktu_mulai`, `waktu_selesai`, `status`.                    | Tergantung S1-T01         | ☐ |
|      | S1-T03  | Pembuatan Model Data (Freezed)            | Buat model `Lapangan` & `Pemesanan` menggunakan Freezed.                                                                                     | Tergantung S1-T02         | ☐ |
| 3-4  | S1-T04  | Implementasi Repository & DAO (Lapangan)  | CRUD untuk lapangan (walau statis dulu: futsal, badminton, voli).                                                                            | Tergantung S1-T02, S1-T03 | ☐ |
|      | S1-T05  | UI: Halaman Daftar Lapangan               | Tampilkan daftar lapangan. Pilih lapangan → navigasi ke halaman jadwal.                                                                      | Tergantung S1-T04         | ☐ |
|      | S1-T06  | State Management (Riverpod) Lapangan      | Provider untuk lapangan (stream list lapangan).                                                                                              | Tergantung S1-T04, S1-T05 | ☐ |
| 5-7  | S1-T07  | Implementasi Repository & DAO (Pemesanan) | CRUD booking (buat, update status, delete). Query jadwal lapangan.                                                                           | Tergantung S1-T02, S1-T03 | ☐ |
|      | S1-T08  | UI: Form Tambah Pemesanan                 | Form pilih lapangan, tanggal & jam (DatePicker/TimePicker).                                                                                  | Tergantung S1-T06, S1-T07 | ☐ |
|      | S1-T09  | State Management (Riverpod) Pemesanan     | Provider untuk mengelola booking (buat, update, cancel).                                                                                     | Tergantung S1-T08         | ☐ |
| 8-9  | S1-T10  | UI: Halaman Jadwal Lapangan               | List booking berdasarkan tanggal. Bisa filter per lapangan.                                                                                  | Tergantung S1-T07         | ☐ |
|      | S1-T11  | State Management (Riverpod) Jadwal        | Stream booking → auto update jadwal.                                                                                                         | Tergantung S1-T10         | ☐ |
| 10   | S1-T12  | Sprint Review, Testing & Refactoring      | Uji alur utama: pilih lapangan → buat booking → lihat jadwal → cancel/selesai booking.                                                       | Semua task                | ☐ |

Sprint 2: Penyempurnaan UI, Status, & Navigasi (Durasi: 2 Minggu)

Tujuan Sprint: Menyempurnakan UI/UX, menambahkan status booking (aktif, dibatalkan, selesai), navigasi antar halaman, dan ringkasan jadwal.
| Hari | Task ID | Tugas                               | Keterangan & Dependencies                                                        | Status              |   |
| ---- | ------- | ----------------------------------- | -------------------------------------------------------------------------------- | ------------------- | - |
| 1-2  | S2-T01  | Implementasi Navigasi (GoRouter)    | Definisikan route: `/`, `/lapangan`, `/jadwal/:id`, `/booking`.                  | Tergantung Sprint 1 | ☐ |
|      | S2-T02  | Logika Bisnis: Status Booking       | Tambah field status (`aktif`, `selesai`, `dibatalkan`) di booking.               | Tergantung S1-T07   | ☐ |
|      | S2-T03  | UI: Update Status Booking           | Tambah tombol aksi: Cancel Booking / Tandai Selesai.                             | Tergantung S2-T02   | ☐ |
| 3-5  | S2-T04  | Logika Filter Jadwal                | Filter jadwal per hari/minggu.                                                   | Tergantung S1-T11   | ☐ |
|      | S2-T05  | UI: Komponen Filter Jadwal          | TabBar (Harian, Mingguan).                                                       | Tergantung S2-T04   | ☐ |
|      | S2-T06  | UI: Grup Jadwal per Tanggal         | Tampilkan header tanggal di list booking.                                        | Tergantung S1-T10   | ☐ |
| 6-7  | S2-T07  | Penyempurnaan UI/UX Global          | Tema warna sederhana, ikon, konsistensi tampilan.                                | Semua task UI       | ☐ |
|      | S2-T08  | Penanganan Error di UI              | Pesan error saat gagal booking/cancel.                                           | Tergantung S1-T07   | ☐ |
|      | S2-T09  | Data Default (Seeding)              | Isi lapangan default: futsal, badminton, voli.                                   | Tergantung S1-T04   | ☐ |
| 8-9  | S2-T10  | Pengujian End-to-End                | Alur lengkap: pilih lapangan → booking → lihat jadwal → filter → cancel/selesai. | Semua task          | ☐ |
|      | S2-T11  | Persiapan Build & Rilis             | Tambah ikon app, splash screen.                                                  | -                   | ☐ |
| 10   | S2-T12  | Sprint Review, Retrospective & Demo | Demo MVP ke stakeholder, kumpulkan feedback.                                     | Semua task          | ☐ |

Catatan Tambahan

Testing paralel: Unit test di Repository & Provider sebaiknya jalan bareng dengan dev.

Backlog: Fitur lanjutan (multi-user login, pembayaran online, notifikasi push, dsb.) masuk backlog sprint berikutnya setelah MVP stabil.