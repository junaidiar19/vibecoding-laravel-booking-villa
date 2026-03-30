# Pertemuan 7: Integrasi 3rd-Party Chatbot & Pengujian Otomatis (Automated Testing)

*Fokus: Menambahkan aspek interaktivitas real-time (Live Chat) untuk meningkatkan persentase penjualan (konversi) dan menciptakan jaring pengaman kode melalui Automated Testing.*

---

## 1. Integrasi Layanan Bantuan Interaktif (Tawk.to)

Sebuah aplikasi pemesanan properti rentan terhadap batalnya transaksi (*cart abandonment*) karena keraguan fasilitas atau jadwal. Menyediakan saluran obrolan (*chat*) adalah standar industri untuk mengatasi hal ini.

### A. Strategi Efisiensi (Pihak Ketiga vs Rancang Modul Sendiri)
Membangun fitur obrolan *real-time* dari nol (melibatkan teknologi WebSockets, Redis, Vue.js/React, dan database chat) memakan alokasi waktu ekstensif yang memecah fokus dari MVP. Pemanfaatan layanan terintegrasi (*plug-and-play*) seperti **tawk.to** memangkas komplikasi ini menjadi nol.

### B. Menyematkan Script ke Layar Antarmuka
Langkah teknis integrasi layanan ini amatlah sederhana:
1. Registrasi pada dasbor tawk.to dan salin kode *JavaScript Snippet* yang disediakan.
2. Tempatkan kode tersebut di dalam berkas tata letak (layout) global aplikasi, yakni pada rute direktori `resources/views/layouts/app.blade.php`.
3. Posisikan blok teks *script* tepat sebelum elemen penutup `</body>` untuk mencegah sumbatan render visual (blokade pembacaan DOM layar atas).

Kini, *floating button* asisten bantuan sudah melayang di seluruh rute halaman portal Anda untuk menyapa pengunjung aktif.

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
