# Pertemuan 7: Strategi Backup Database & Pengujian Otomatis (Automated Testing)

*Fokus: Mengamankan data riwayat transaksi operasional melalui backup otomatis dan menciptakan jaring pengaman kode melalui Automated Testing.*

---

## 1. Strategi Backup Otomatis untuk Database Transaksi

Sebutan "data adalah aset utama" amat berlaku di sistem e-commerce. Apabila server mengalami kerusakan (kegagalan perangkat keras) atau terjadi manipulasi serangan fatal, satu-satunya penyelamat bisnis Anda adalah presensi fail *Backup* (Cadangan) data pemesanan.

### A. Pembuatan Komando Pencadangan (Artisan Command)
Membangun instruksi yang memerintahkan mesin database merekam dirinya sendiri:
- Menginstruksikan Vibe Coding IDE untuk membuat artisan command: `php artisan make:command BackupDatabase`.
- Menyuntikkan fungsi agar command lokal menjalankan eksekusi utilitas sistem operasi semacam `mysqldump` demi mengekspor keseluruhan struktur MySQL menjadi fail format `.sql`.
- Menamakan fail output tersebut secara dinamis dengan penanda waktu *(timestamp)* seperti `backup-2026-03-30.sql` agar tidak terjadi penimpaan *(overwrite)* dokumen historis.

### B. Penjadwalan Otomatis (Task Scheduling)
Langkah teknis integrasi otomatisasi menggunakan layanan dasar Laravel:
1. Buka konfigurasi kernel konsol di dalam `app/Console/Kernel.php` (atau rute setara di Laravel versi 11/12).
2. Manfaatkan *method* `schedule` untuk mendaftarkan command ciptaan Anda: `$schedule->command('app:backup-database')->dailyAt('02:00');`.
3. Pasangkan ke *CRON Job* server (*Linux*) agar mesin Laravel aktif memeriksa waktunya setiap menit.

Kini, Anda bisa tidur nyenyak karena rekam jejak pembayaran *(Booking)* tersimpan otomatis ke dalam fail cadangan *(Archive)* setiap dini hari saat trafik pengujung sedang sepi.

---

## 2. Esensi Pengujian Sistem Otomatis (PestPHP Testing)

Menelurkan produk aplikasi menggunakan IDE Vibe Coding (seperti Google Antigravity) ibarat mengendarai mobil dengan kecepatan sangat tinggi. Jika terjadi perbaikan (*refactoring*) algoritma di kemudian hari yang merusak logika fungsi lama, menemukan eror secara penelusuran satu per satu akan memakan waktu dua kali lipat lebih lama.

*Automated Testing* merupakan sabuk pengaman Anda.

### A. Evaluasi Modul Minor (*Unit Validation Test*)
Metode yang berfokus memeriksa fungsi satuan, terutama pada formula perhitungan diskon atau pajak.
- **Kasus Pengujian:** Jika aplikasi memproses inap 3 malam pada harga properti Rp1.000.000,- ditambah nominal pajak 11%, nilai bersih pengujian harus absolut bernilai 3.330.000.
- **Implementasi (Vibe Coding):** *"Buka file `tests/Unit/KalkulasiTest.php` melalui ekstensi Pest. Buat sebuah test yang mengirim parameter hari inap sejumlah '3' dan nominal `price` 1.000.000 pada kelas Servis Kalkulator Anda. Gunakan klaim ekspektasi metode ekual `expect($total)->toEqual(3330000)`."*

### B. Evaluasi Integrasi Menyeluruh (*Feature Integration Test*)
Tanda mutlak keberhasilan sebuah siklus e-commerce adalah perjalanan taktis pemesanan secara makro tanpa perlu kita mensimulasikan klik kursor manual (*browser clicking*).

**Skenario Proteksi Cerdas Vibe Coding:**
> *"Di dalam `tests/Feature/SiklusBookingTest.php`, susun satu rekam jejak pengujian komprehensif. Pertama, nyalakan `RefreshDatabase`. Kedua, actingAs pengguna login acak menggunakan `User::factory()->create()`. Ketiga, perintahkan sistem menembakkan *(hit)* route POST `/bookings` dengan menyertakan Payload JSON data `villa_id` dan struktur waktu inap sebulan ke depan. Terakhir, buat asersi (Assertion) yang memeriksa apakah balasan akhir server bernilai HTTP status 200, lalu verifikasi `assertDatabaseHas` untuk memvalidasi kemunculan nama pengguna bersangkutan ke dalam tabel rekaman."*

Apabila skrip baris pengujian *Pest* berlari dalam instrumen konsole Terminal menyematkan centang hijau seluruhnya, tandanya aplikasi Booking Villa ini telah siap rilis (*Production-Ready*).
