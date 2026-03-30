# Pertemuan 5: Pengamanan Logika Bisnis (Security & Validasi)

*Fokus: Melindungi aplikasi dari kerentanan umum dan mencegah manipulasi (eksploitasi logika) terkait integritas data sistem pemesanan.*

---

## 1. Pemahaman Dasar Celah Keamanan (Vulnerability Framework)

Sekalipun Anda mengandalkan AI via Vibe Coding untuk menghasilkan kode secepat kilat, AI bisa saja menulis kode yang "berfungsi" namun belum tentu "aman". Sangat krusial membekali diri dengan tinjauan keamanan untuk memvalidasi output IDE Anda.

### A. Proteksi Bawaan Framework Laravel
Beruntungnya, integrasi keamanan standar telah dibawa secara otomatis *(built-in)* oleh ekosistem Laravel:
- **CSRF Token:** Anda bisa menyuruh AI untuk tidak pernah tertinggal menulis penanda wajib `@csrf` di formulir *POST*. Token kriptografi ini mencegah eksploitasi peretasan kueri ilegal dari situs kloningan (pihak luar).
- **XSS (Cross-Site Scripting):** Penjelasan singkat bagaimana engine Blade `{{ $variabel }}` mengamankan sintaks dengan enkripsi *HTML Entity*, mencegah *scripting* berbahaya yang mencoba dieksekusi oleh peretas (seperti link jahat bertopeng tautan).
- **SQL Injection Guard:** Bahwa metode *Query Builder* dan *Eloquent ORM* otomatis memfasilitasi parameter PDO di belakang layar, sehingga aplikasi kebal dari injeksi query manipulasi.

---

## 2. Proteksi Eksploitasi: Data Tampering (Integritas Harga)

Ini adalah celah terfatal di mayoritas platform pemesanan amatir. Sering kali para pengembang sekedar "melempar" angka dari variabel formulir di *Frontend* ke dalam logika pemrosesan keranjang tagihan.

### A. Anatomi Manipulasi Harga via "Inspect Element"
- **Skenario Retas:** Input tersembunyi seperti `<input name="total_price" value="2500000">` amat mudah dibongkar dalam mode pengembang (browser), diganti nilainya menjadi `"10"`, kemudian dikirimkan ke server.
- Jika Controller Laravel menangkap buta variabel `request->total_price` lalu mem- *by-pass* penyimpanannya ke tabel `payments` atau penyedia Gateway perbankan, hacker sukses mengamankan produk jutaan tersebut dengan uang sepuluh rupiah. 

### B. Validasi Harga Searah (Server-Side Calculation)
Solusi tuntasnya, jangan pernah merekam perhitungan (kalkulasi uang) dari sumber klaim perangkat pengguna (*Frontend*).
Sesuai penjabaran pada **Sesi 4 sebelumnya**, pastikan algoritma perkalian tagihan ditarik dan bersumber mutlak dari angka asli basis data murni (yakni tabel `villas` di kolom `price_per_night`).

---

## 3. Resolusi Konkurensi: Menangkal Insiden "Double Booking"

Mimpi buruk setiap operasional perhotelan adalah satu ruangan terisi pesanan tumpang tindih untuk tanggal yang ekuivalen (oleh pengunjung ganda di momen eksekusi bersamaan). Secara teknis rekayasa piranti lunak, ini disebut fenomena **Race Condition**.

### A. Penyelesaian Logika *Overlap Data* (Validasi Tanggal Tabrakan)
Sebelum aplikasi menyimpan *record* ke tabel *Bookings*, IDE Cerdas Antigravity harus dipandu memasukkan logika inspeksi tanggal (Tumpang Tindih):

**Opsi Prompt yang Disarankan:**
> *"Di dalam validasi fungsi pemesanan BookingController, tambahkan satu blok aturan Eloquent khusus sebelum method save. Periksa apakah `villa_id` terkait telah dibooking oleh pengunjung lain (syarat: status 'paid' atau 'pending'). Cari irisan tanggal di mana parameter `$request->check_in` ATAU `$request->check_out` jatuh di antara rentang tanggal pesanan aktif di database. Jika ditemukan benturan *(overlapping/konflik)* hari menginap, batalkan eksekusi lalu *return error* bahwa properti tidak tersedia. Gunakan metode kueri 'whereBetween'."*

### B. Konsep Pelindung Basis Data (Pessimistic Locking / Level Advance)
(Sebagai pendalaman): Sekilas penerapan pelindung database `LockForUpdate()` atau mode status "Menahan Proses (Row Lock)" pada baris di mana dua user seolah mengklik tombol pada selisih kedipan mili-detik (mils) yang tepat bersamaan, menyadarkan bahwa relasi basis data akan melahirkan skema mengantre (antrean linear) yang adil hingga sistem mengakhiri (commit) transaksinya satu per satu tanpa benturan sama sekali.
