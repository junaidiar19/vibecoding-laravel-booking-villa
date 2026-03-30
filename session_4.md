# Pertemuan 4: Algoritma Pencarian & Alur Booking Utama 

*Fokus: Membangun sistem pencarian properti yang dinamis dan menyusun logika siklus pemesanan aplikasi (kalkulasi harga serta penyimpanan transaksi).*

---

## 1. Sistem Pencarian Villa Berbasis Eloquent

Aplikasi dengan ratusan entri data wajib memiliki fitur pencarian untuk memudahkan pengunjung. Karena pendekatan kita adalah aplikasi tradisional (Server-Side), kita akan menggunakan Form berbasis *GET request* dan mengolah argumennya pada Controller.

### A. Merancang Struktur Input (Form Pencarian)
Sebuah filter dasar minimal terdiri dari:
- **Keyword (Teks):** Mencari berdasarkan nama atau lokasi properti.
- **Batasan Kapasitas (Angka):** Memfilter villa khusus keluarga besar atau sebaliknya.
- **Rentang Harga (Angka):** Penyortiran harga per malam minimum dan maksimum.

### B. Implementasi Logika dengan Vibe Coding
Merakit *query* database (`where`, `orWhere`, `whereBetween`) secara manual rentan terhadap salah koding atau celah kemanan (SQL Injection jika tidak ditangani via PDO). Kita akan mendelegasikan perintah ini ke Google Antigravity.

**Opsi Prompt yang Disarankan:**
> *"Perbarui method `index` di `VillaController`. Tangkap variabel request opsional (query param) seperti: `keyword`, `min_capacity`, `min_price`, dan `max_price`. Modifikasi query panggilannya; jika param keyword diberikan, saring menggunakan Eloquent `where('name', 'like', '%...%')`. Integrasikan semua pencariannya menggunakan `when()` Eloquent untuk kode yang lebih bersih, lalu tutup dengan `paginate(10)`."*

Tugas Anda hanya me- *review* struktur kode yang telah di- *generate* dan mengujinya lewat komponen UI *Frontend*.

---

## 2. Alur Pembentukan Pesanan (Booking Lifecycle)

Saat pengunjung mengklik tombol "Pesan" di profil rincian properti, mereka memasuki fase retensi/transaksi aplikasi Anda. Di fase ini akurasi data finansial sangat diperlukan.

### A. Kalkulasi Aritmatika Tanggal
Kesalahan umum adalah membiarkan *Front-end* mendelegasikan total harga bulat, yang kerap berujung penipuan (Tampering). Anda wajib menghitung total secara *Server-side*:
1. Mengumpulkan input `check_in_date` dan `check_out_date`.
2. Menghapus (selisih) kedua tanggal tersebut dengan library **Carbon** milik Laravel untuk mendapati total *malam menginap*.
3. Dikalikan harga dasar properti (`price_per_night`).

### B. Status Pemesanan (*State Management*)
Sistem pemesanan membutuhkan status (State). Jika transaksi sukses tercipta di tabel `bookings`, ia **tidak boleh** langsung berstatus `"paid"` (Lunas), melainkan kita menggunakan status awal default:
- **`pending`**: Tagihan sedang menunggu pembayaran.
- **`paid`**: Diverifikasi oleh sistem atau admin.
- **`canceled` / `expired`**: Melebihi batas waktu pelunasan.

---

## 3. Eksekusi Pembentukan Endpoint Booking

Sesi ini diakhiri dengan instruksi pembuatan *endpoint/controller function* yang bertugas merekam pesanan tamu ke dalam database MySQL.

**Opsi Prompt yang Disarankan:**
> *"Buat method `store` di `BookingController`. Lakukan validasi form: `check_in_date` tidak boleh mundur (minimal hari ini) dan `check_out_date` harus sesudah check in. Ambil durasi hari menggunakan Carbon, dan kalikan secara sistem backend dengan relasi properti `$villa->price_per_night`. Insert hasil penjumlahannya ke tabel `bookings` berikut id pengguna (`Auth::id`) dengan status default 'pending'. Redirect aplikasi ke halaman keranjang detail_tagihan."*

Alur ini mengamankan fondasi pesanan *(Cart)* sebelum kita menghubungkannya ke sistem perbankan nyata di Pertemuan ke-6 nanti.
