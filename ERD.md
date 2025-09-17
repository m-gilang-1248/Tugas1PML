# ERD: Aplikasi Pemesanan Lapangan Olahraga
## Entitas & Atribut beserta detail dan hubungan antar collection:

```mermaid
erDiagram
    USERS ||--o{ BOOKINGS : "makes"
    USERS ||--o{ BLOCKED_SLOTS : "blocks"
    USERS ||--|{ SPORT_CENTERS : "manages"

    SPORT_CENTERS ||--|{ USERS : "has_admin"
    SPORT_CENTERS ||--o{ FIELDS : "has"
    SPORT_CENTERS ||--o{ BOOKINGS : "receives_for"
    SPORT_CENTERS ||--o{ BLOCKED_SLOTS : "has_blocked_slots_for"

    FIELDS ||--|{ SPORT_CENTERS : "belongs_to"
    FIELDS ||--o{ BOOKINGS : "is_booked_in"
    FIELDS ||--o{ BLOCKED_SLOTS : "has_blocked_slots"

    BOOKINGS ||--|| USERS : "booked_by_player"
    BOOKINGS ||--|| FIELDS : "for_field"
    BOOKINGS ||--|| SPORT_CENTERS : "at_center"

    BLOCKED_SLOTS ||--|| USERS : "blocked_by_admin"
    BLOCKED_SLOTS ||--|| FIELDS : "for_field"
    BLOCKED_SLOTS ||--|| SPORT_CENTERS : "at_center"

    USERS {
        string user_id PK "User ID (Appwrite $id)"
        string name "Nama Lengkap"
        string email "Email (unique)"
        string phone "Nomor Telepon (ops)"
        object prefs "Preferensi Pengguna"
        string prefs_role "Peran (player, sc_admin, super_admin)"
        string prefs_assignedCenterId "FK ke SPORT_CENTERS.$id (jika sc_admin)"
        boolean emailVerification "Status Verifikasi Email"
        boolean status "Status Akun (aktif/tidak aktif)"
        datetime createdAt "Waktu Pembuatan"
        datetime updatedAt "Waktu Update"
    }

    SPORT_CENTERS {
        string center_id PK "SC ID (Appwrite $id)"
        string sc_name "Nama SC"
        string sc_address "Alamat SC"
        string sc_city "Kota SC (indexed)"
        string sc_contact_phone "Kontak Telepon SC (ops)"
        string sc_contact_email "Kontak Email SC (ops)"
        string sc_description "Deskripsi SC (ops)"
        string sc_operating_hours_open "Jam Buka (HH:MM)"
        string sc_operating_hours_close "Jam Tutup (HH:MM)"
        string sc_main_photo_url "URL Foto Utama (ops)"
        string_array sc_additional_photos_urls "URL Foto Tambahan (ops)"
        string_array sc_facilities "Fasilitas (ops)"
        string sc_status "Status SC (pending, active, inactive)"
        string sc_admin_team_id "ID Team Admin SC di Appwrite"
        datetime createdAt "Waktu Pembuatan"
        datetime updatedAt "Waktu Update"
    }

    FIELDS {
        string field_id PK "Field ID (Appwrite $id)"
        string center_id FK "FK ke SPORT_CENTERS.$id (indexed)"
        string field_name "Nama Lapangan"
        string sport_type "Jenis Olahraga (indexed)"
        string field_type "Tipe Lapangan (indoor/outdoor, ops)"
        string floor_type "Jenis Lantai (ops)"
        float price_per_hour "Harga per Jam"
        string field_description "Deskripsi Lapangan (ops)"
        string_array field_photos_urls "URL Foto Lapangan (ops)"
        boolean is_active "Status Aktif Lapangan"
        datetime createdAt "Waktu Pembuatan"
        datetime updatedAt "Waktu Update"
    }

    BOOKINGS {
        string booking_id PK "Booking ID (Appwrite $id)"
        string player_user_id FK "FK ke USERS.$id (indexed)"
        string center_id FK "FK ke SPORT_CENTERS.$id (indexed)"
        string field_id FK "FK ke FIELDS.$id (indexed)"
        string booking_date "Tanggal Main (YYYY-MM-DD, indexed)"
        string start_time "Waktu Mulai (HH:MM, indexed)"
        string end_time "Waktu Selesai (HH:MM, indexed)"
        float duration_hours "Durasi (jam)"
        float total_price "Total Harga"
        string booking_status "Status Booking (pending_payment, confirmed, etc., indexed)"
        string payment_method "Metode Bayar (ops)"
        string payment_proof_url "URL Bukti Bayar (ops)"
        string player_notes "Catatan Pemain (ops)"
        string sc_notes "Catatan SC (ops)"
        string booked_by_role "Dipesan oleh (player/sc_admin)"
        datetime createdAt "Waktu Pembuatan Booking"
        datetime updatedAt "Waktu Update Booking"
    }

    BLOCKED_SLOTS {
        string blocked_slot_id PK "Blocked Slot ID (Appwrite $id)"
        string center_id FK "FK ke SPORT_CENTERS.$id (indexed)"
        string field_id FK "FK ke FIELDS.$id (indexed)"
        string block_date "Tanggal Blokir (YYYY-MM-DD, indexed)"
        string start_time "Waktu Mulai Blokir (HH:MM, indexed)"
        string end_time "Waktu Selesai Blokir (HH:MM, indexed)"
        string reason "Alasan Blokir (ops)"
        string blocked_by_user_id FK "FK ke USERS.$id"
        datetime createdAt "Waktu Pembuatan"
        datetime updatedAt "Waktu Update"
    }
```

**Aturan Umum Permissions:**
*   **`role:super_admin`**: Biasanya memiliki CRUD penuh pada semua collection untuk keperluan administrasi.
*   **`team:sc_[centerId]_admins`**: Tim yang berisi ID user admin dari SC tertentu. Digunakan untuk memberikan akses CRUD ke data milik SC tersebut.
*   **`user:[userId]`**: Memberikan akses kepada pengguna spesifik (pemilik dokumen).
*   **`role:player`**: Memberikan akses baca umum atau hak buat terbatas.

---

**5.1. Collection: `users`**
*   **Tujuan:** Menyimpan data semua pengguna (Pemain, Admin SC, Super Admin).
*   **Atribut:**
    *   `$id` (string, PK, otomatis oleh Appwrite): ID unik pengguna.
    *   `$createdAt` (datetime, otomatis): Waktu pembuatan.
    *   `$updatedAt` (datetime, otomatis): Waktu update terakhir.
    *   `name` (string, wajib): Nama lengkap pengguna.
    *   `email` (string, wajib, unik, indexed): Alamat email pengguna (untuk login).
    *   `emailVerification` (boolean, otomatis): Status verifikasi email.
    *   `password` (string, otomatis, hashed): Password pengguna (dikelola Appwrite).
    *   `phone` (string, opsional): Nomor telepon pengguna.
    *   `prefs` (object, otomatis): Preferensi pengguna (bisa berisi `role` dan `assignedCenterId`).
        *   `role` (string, wajib, enum: `player`, `pending_sc_admin`, `sc_admin`, `super_admin`): Peran pengguna dalam sistem.
        *   `assignedCenterId` (string, opsional, FK ke `sport_centers.$id`): Diisi jika `role` adalah `sc_admin`, menunjukkan SC yang dikelola.
    *   `status` (boolean, otomatis): Status akun (aktif/tidak aktif).
*   **Permissions Dokumen Default:**
    *   Read: `role:super_admin`, `user:$id`
    *   Update: `role:super_admin`, `user:$id` (Pengguna bisa update profil sendiri)
    *   Delete: `role:super_admin`
*   **Relasi:**
    *   `prefs.assignedCenterId` -> `sport_centers.$id` (Satu Admin SC mengelola satu SC - untuk MVP).

---

**5.2. Collection: `sport_centers` (SC)**
*   **Tujuan:** Menyimpan data detail setiap Sports Center (tenant).
*   **Atribut:**
    *   `$id` (string, PK, otomatis): ID unik SC.
    *   `$createdAt` (datetime, otomatis): Waktu pembuatan.
    *   `$updatedAt` (datetime, otomatis): Waktu update terakhir.
    *   `sc_name` (string, wajib, indexed): Nama resmi Sports Center.
    *   `sc_address` (string, wajib): Alamat lengkap SC.
    *   `sc_city` (string, wajib, indexed): Kota lokasi SC (untuk pencarian).
    *   `sc_contact_phone` (string, opsional): Nomor telepon kontak SC.
    *   `sc_contact_email` (string, opsional): Email kontak SC.
    *   `sc_description` (string, opsional, teks panjang): Deskripsi tentang SC.
    *   `sc_operating_hours_open` (string, wajib, format "HH:MM"): Jam buka SC.
    *   `sc_operating_hours_close` (string, wajib, format "HH:MM"): Jam tutup SC.
    *   `sc_main_photo_url` (string, opsional): URL ke foto utama SC (dari Appwrite Storage).
    *   `sc_additional_photos_urls` (array of strings, opsional): Daftar URL ke foto tambahan SC.
    *   `sc_facilities` (array of strings, opsional): Daftar fasilitas (misal: "Parkir", "Toilet", "Kantin").
    *   `sc_status` (string, wajib, enum: `pending_approval`, `active`, `inactive`, `suspended`, indexed): Status SC.
    *   `sc_admin_team_id` (string, wajib): ID Team Appwrite (`sc_[$id]_admins`) yang berisi admin(s) untuk SC ini.
*   **Permissions Dokumen Default:**
    *   Read: `role:super_admin`, `role:player`, `team:[sc_admin_team_id]`
    *   Create: `role:super_admin` (atau Function yang dipicu oleh `pending_sc_admin` yang disetujui)
    *   Update: `role:super_admin`, `team:[sc_admin_team_id]`
    *   Delete: `role:super_admin`
*   **Relasi:**
    *   Memiliki banyak `fields`.
    *   Dikelola oleh pengguna dalam `team:[sc_admin_team_id]`.

---

**5.3. Collection: `fields` (Lapangan)**
*   **Tujuan:** Menyimpan data detail setiap lapangan milik Sports Center.
*   **Atribut:**
    *   `$id` (string, PK, otomatis): ID unik lapangan.
    *   `$createdAt` (datetime, otomatis): Waktu pembuatan.
    *   `$updatedAt` (datetime, otomatis): Waktu update terakhir.
    *   `center_id` (string, wajib, FK ke `sport_centers.$id`, indexed): ID SC pemilik lapangan ini.
    *   `field_name` (string, wajib): Nama lapangan (misal: "Lapangan Futsal A", "Badminton Indoor 1").
    *   `sport_type` (string, wajib, enum: `futsal`, `badminton`, `basketball`, `volleyball`, `tennis`, indexed): Jenis olahraga utama lapangan.
    *   `field_type` (string, opsional, enum: `indoor`, `outdoor`): Jenis lapangan (indoor/outdoor).
    *   `floor_type` (string, opsional): Jenis lantai (misal: "Sintetis", "Vinyl", "Semen").
    *   `price_per_hour` (number, wajib, float): Harga sewa per jam.
    *   `field_description` (string, opsional): Deskripsi singkat lapangan.
    *   `field_photos_urls` (array of strings, opsional): Daftar URL ke foto-foto lapangan.
    *   `is_active` (boolean, wajib, default: true): Status aktif/tidak aktif lapangan.
*   **Permissions Dokumen Default:**
    *   Read: `role:super_admin`, `role:player`, `team:[center_id]_admins` (Asumsi Team ID mengikuti pola `sc_[center_id]_admins`)
    *   Create: `role:super_admin`, `team:[center_id]_admins`
    *   Update: `role:super_admin`, `team:[center_id]_admins`
    *   Delete: `role:super_admin`, `team:[center_id]_admins`
*   **Relasi:**
    *   Dimiliki oleh satu `sport_centers` (via `center_id`).
    *   Memiliki banyak `bookings`.
    *   Memiliki banyak `blocked_slots`.

---

**5.4. Collection: `bookings` (Pemesanan)**
*   **Tujuan:** Menyimpan data setiap transaksi pemesanan lapangan.
*   **Atribut:**
    *   `$id` (string, PK, otomatis): ID unik pemesanan.
    *   `$createdAt` (datetime, otomatis): Waktu pemesanan dibuat.
    *   `$updatedAt` (datetime, otomatis): Waktu update terakhir.
    *   `player_user_id` (string, wajib, FK ke `users.$id`, indexed): ID pengguna (pemain) yang memesan.
    *   `center_id` (string, wajib, FK ke `sport_centers.$id`, indexed): ID SC tempat lapangan dipesan (denormalisasi untuk query).
    *   `field_id` (string, wajib, FK ke `fields.$id`, indexed): ID lapangan yang dipesan.
    *   `booking_date` (string, wajib, format "YYYY-MM-DD", indexed): Tanggal bermain.
    *   `start_time` (string, wajib, format "HH:MM", indexed): Waktu mulai bermain.
    *   `end_time` (string, wajib, format "HH:MM", indexed): Waktu selesai bermain.
    *   `duration_hours` (number, wajib, float): Durasi sewa dalam jam.
    *   `total_price` (number, wajib, float): Total harga pemesanan.
    *   `booking_status` (string, wajib, enum: `pending_payment`, `confirmed`, `cancelled_by_player`, `cancelled_by_sc`, `completed`, `payment_rejected`, `waiting_for_sc_confirmation`, indexed): Status pemesanan.
    *   `payment_method` (string, opsional, enum: `on_the_spot`, `bank_transfer`): Metode pembayaran (untuk MVP).
    *   `payment_proof_url` (string, opsional): URL ke bukti pembayaran jika `bank_transfer`.
    *   `player_notes` (string, opsional): Catatan dari pemain.
    *   `sc_notes` (string, opsional): Catatan dari admin SC terkait booking ini.
    *   `booked_by_role` (string, wajib, enum: `player`, `sc_admin`): Siapa yang membuat booking (pemain atau admin SC untuk booking manual).
*   **Permissions Dokumen Default:**
    *   Read: `role:super_admin`, `user:[player_user_id]`, `team:[center_id]_admins`
    *   Create: `role:player`, `team:[center_id]_admins`
    *   Update: `role:super_admin`, `user:[player_user_id]` (hanya untuk status tertentu seperti `cancelled_by_player`), `team:[center_id]_admins` (untuk konfirmasi, pembatalan oleh SC)
    *   Delete: `role:super_admin` (pembatalan keras, jarang digunakan)
*   **Relasi:**
    *   Dibuat oleh satu `users` (pemain).
    *   Merujuk ke satu `fields` dan satu `sport_centers`.

---

**5.5. Collection: `blocked_slots` (Slot Waktu Diblokir Admin SC)**
*   **Tujuan:** Menyimpan informasi slot waktu yang diblokir oleh Admin SC (misal untuk maintenance, acara internal).
*   **Atribut:**
    *   `$id` (string, PK, otomatis): ID unik slot diblokir.
    *   `$createdAt` (datetime, otomatis): Waktu pembuatan.
    *   `$updatedAt` (datetime, otomatis): Waktu update terakhir.
    *   `center_id` (string, wajib, FK ke `sport_centers.$id`, indexed): ID SC.
    *   `field_id` (string, wajib, FK ke `fields.$id`, indexed): ID lapangan yang slotnya diblokir.
    *   `block_date` (string, wajib, format "YYYY-MM-DD", indexed): Tanggal slot diblokir.
    *   `start_time` (string, wajib, format "HH:MM", indexed): Waktu mulai blokir.
    *   `end_time` (string, wajib, format "HH:MM", indexed): Waktu selesai blokir.
    *   `reason` (string, opsional): Alasan pemblokiran slot.
    *   `blocked_by_user_id` (string, wajib, FK ke `users.$id`): ID Admin SC yang melakukan blokir.
*   **Permissions Dokumen Default:**
    *   Read: `role:super_admin`, `role:player` (aplikasi akan query ini untuk menentukan ketersediaan), `team:[center_id]_admins`
    *   Create: `role:super_admin`, `team:[center_id]_admins`
    *   Update: `role:super_admin`, `team:[center_id]_admins`
    *   Delete: `role:super_admin`, `team:[center_id]_admins`
*   **Relasi:**
    *   Merujuk ke satu `fields` dan satu `sport_centers`.
    *   Dibuat oleh satu `users` (Admin SC).

---
