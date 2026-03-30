# Pertemuan 2: Desain Database & Setup Proyek

*Fokus: Meletakkan pondasi data yang solid untuk aplikasi Booking Villa menggunakan metodologi Vibe Coding.*

---

## 1. Arsitektur Database (Merancang ERD)

Sebelum menggunakan Google Antigravity untuk membuat *migration*, kita harus menentukan skema strukturnya. Aplikasi Booking Villa MVP kita akan menggunakan **4 Tabel Utama**:

### A. Rincian Tabel
1. **`users` (Tabel Tamu/Admin)**
   - Berisi data autentikasi: `nama`, `email`, `password`, dan `role` (contoh: 'guest' atau 'admin').
2. **`villas` (Tabel Properti)**
   - Berisi data properti: `name` (Nama Villa), `description`, `price_per_night` (Harga per malam), `capacity` (Jumlah orang maksimal), `address` (Lokasi), dan `thumbnail_url`.
3. **`bookings` (Tabel Pesanan/Transaksi)**
   - Menghubungkan Tamu dengan Villa: `user_id`, `villa_id`, `check_in_date`, `check_out_date`, `total_price`, dan `status` (pending, paid, canceled).
4. **`payments` (Tabel Pembayaran terintegrasi Midtrans)**
   - Mencatat rincian spesifik transaksi: `booking_id`, `midtrans_transaction_id`, `payment_method`, dan `payment_status`.

### B. Memahami Relasi (Relationship)
Dalam Laravel (Eloquent), tabel ini saling berkaitan:
- **One-to-Many:** Satu orang pengguna (`User`) bisa memesan banyak properti (`Bookings`). Satu tipe `Villa` bisa dipesan berkali-kali (`Bookings`).
- **One-to-One:** Satu transaksi `Booking` terkait dengan satu pencatatan `Payment`.

---

## 2. Eksekusi "Vibe Coding" via Google Antigravity

Membuat skema 4 tabel lengkap beserta relasinya secara manual bisa memakan waktu cukup lama dan rawan kesalahan pengetikan (`typo`). Dengan metode Vibe Coding, kita menyerahkan tugas repetitif ini ke IDE.

### A. Scaffolding Project Baru
Instruksikan perintah berikut di terminal untuk membuat dasar framework baru:
`laravel new villa-booking-app --breeze`  
*(Kita menggunakan `--breeze` agar fitur login/register otomatis terinstall).*

### B. Contoh Prompt untuk Generate Database
Setelah project ter-install, buka percakapan dengan Google Antigravity, dan tuliskan instruksi (prompt) yang jelas dan terstruktur seperti berikut:

> *"Saya ingin merancang database aplikasi booking villa. Buatkan file Migration, Model, dan Factory untuk 3 entitas ini: Villa, Booking, dan Payment (User sudah bawaan Breeze).*
> *1. Tabel Villa memiliki kolom price_per_night.*
> *2. Tabel Booking memiliki user_id, villa_id, tanggal check_in, tanggal_check_out, dan status (berikan default 'pending'). Pastikan foreign key menggunakan cascade on delete.*
> *3. Tabel Payment memiliki booking_id. Buatkan juga fungsi relasi Eloquent 'belongsTo' dan 'hasMany' pada setiap file Model-nya."*

**Hasil:**  
IDE akan otomatis membuatkan puluhan file (*migration*, *model*, *factory*) sesuai instruksi. Anda tinggal menjalankan `php artisan migrate` dan meninjau ulang relasi di file *Model* untuk memastikannya tepat.

---

## 3. Pentingnya Naming Conventions (Aturan Penamaan)

Agar Google Antigravity dapat memproses instruksi dengan akurat, Anda **diwajibkan** untuk mengikuti standar penamaan bawaan Laravel (*Convention over Configuration*). 

AI dilatih dengan ribuan repositori proyek Laravel *open-source*. Konsistensi penamaan akan sangat membantu AI memahami konteks kode Anda.

**Garis Besar Aturan Penamaan:**
- **Nama Tabel (Jamak/Plural):** `villas`, `bookings`. (Hindari penamaan pencampuran bahasa seperti `tabel_villa`).
- **Nama Model (Tunggal/Singular & PascalCase):** `Villa`, `Booking`.
- **Foreign Key (snake_case & suffix _id):** Untuk referensi tabel `villas`, gunakan nama kolom `villa_id`. Hindari variasi seperti `id_villa` atau `properti_id`.
- **Method Relasi:** Jika fitur ini menghubungkan satu objek dengan banyak objek lain (seperti satu user memiliki banyak booking), gunakan format jamak seperti `public function bookings()`.

Mematuhi *Naming Conventions* memastikan proses pengembangan dibantu AI menjadi jauh lebih mulus dan minim ralat struktur struktur.
