# Pertemuan 1: Fundamental "Vibe Coding" & Perencanaan Proyek (PRD)

*Fokus: Memahami mindset developer modern dan merumuskan spesifikasi aplikasi agar AI tidak kebingungan.*

---

## 1. Konsep "Vibe Coding"

Di era modern, cara kita menulis kode telah berubah drastis. **Vibe Coding** adalah istilah di mana developer tidak lagi menulis kode secara manual baris-demi-baris, melainkan bertindak sebagai **"Arsitek"** atau **"Mandor"** yang memberikan instruksi (prompts), lalu **"Tukang"** (AI) yang akan mengeksekusinya. 

### A. Pergeseran Peran Programmer (Arsitek & Reviewer)
- **Penjelasan:** Dulu developer menghafal *syntax* dan membaca dokumentasi berjam-jam hanya untuk membuat sebuah fitur standar. Sekarang, peran Anda bergeser untuk lebih fokus memikirkan *Business Logic* (Logika Bisnis). AI yang bertugas mengingat *syntax* Laravel. Tugas Anda selanjutnya adalah me-*review* dan mengetes kode tersebut.
- **Contoh Analogi:** Alih-alih mengetik manual `php artisan make:controller VillaController --resource` lalu mengetik isi fungsinya satu-persatu, Anda cukup meminta ke AI: *"Buatkan VillaController lengkap dengan fungsi CRUD, pastikan menggunakan Form Request Validation dan kembalikan JSON response".*

### B. Pengenalan IDE Google Antigravity
- **Penjelasan:** Google Antigravity bukan sekadar ekstensi *autocomplete* biasa. Ini adalah *agentic IDE* (IDE yang bisa bertindak secara mandiri). Ia bisa melihat struktur file Anda, menjalankan terminal, dan mengedit beberapa file sekaligus berdasarkan instruksi Anda.
- **Contoh Penggunaan:** Kita akan mempraktikkan bagaimana membuka percakapan dengan Antigravity, lalu memintanya: *"Tolong baca file `routes/web.php` dan `VillaController.php`, lalu buatkan Test Case untuk endpoint tersebut."* Antigravity akan mengurus semuanya tanpa Anda harus berpindah-pindah layar/tab (context-switching).

### C. Teknik Prompting Efektif di Laravel
- **Penjelasan:** Kunci keberhasilan *Vibe Coding* ada di bahasa instruksi (Prompt). AI hanya secerdas instruksi yang diterimanya.
- **Contoh BAD Prompt:** ❌ *"Buatkan fitur cari villa."* (Terlalu ambigu. AI akan bingung database-nya apa, parameternya apa, dan return-nya ke mana).
- **Contoh GOOD Prompt:** ✅ *"Buatkan method `search` di `VillaController` untuk Laravel 11. Method ini harus menerima 3 parameter dari request: `keyword` (string, mencari nama villa), `min_price` (integer), dan `max_price` (integer). Gunakan Eloquent dengan metode `where` dan `orWhere`. Return hasilnya ke view `villas.index` lengkap dengan link pagination."*

---

## 2. Produk Inception: Studi Kasus Aplikasi Booking Villa

Sebelum coding, kita harus tahu apa yang mau dibuat. Kita akan mengadopsi prinsip **MVP (Minimum Viable Product)**—membuat versi paling dasar dari aplikasi yang sudah bisa berfungsi dan mendatangkan profit (atau minimal menguji ide).

### Bedah Fitur Esensial (MVP)
Kita membatasi scope aplikasi hanya pada fitur yang *wajib* ada:
1. **Katalog Villa:** Halaman daftar villa (Thumbnail, Nama, Harga/malam, Kapasitas) dan Halaman Detail.
2. **Autentikasi User:** Fitur Login/Register agar sistem tahu *booking* tersebut milik siapa.
3. **Flow Booking:** Form pemesanan (Pilih tanggal *Check-in* & *Check-out*). Pastikan belum ada yang mem-booking di tanggal tersebut.
4. **Checkout & Integrasi Pembayaran:** Halaman keranjang berisi total tagihan, dan terhubung ke Midtrans untuk diproses.

*(Catatan: Fitur "nice-to-have" seperti Review Bintang 5, Filter Radius Maps, atau Multi-bahasa akan ditunda pada tahap ini demi berfokus mencapai MVP).*

---

## 3. Membuat PRD (Product Requirements Document)

Banyak yang berpikir *"Karena pakai AI, kita gak perlu mikir rancangan, langsung suruh aja"*. Ini **SALAH BESAR**. AI butuh kompas. Jika aplikasinya kompleks, AI bisa berhalusinasi. PRD adalah "kompas" tersebut. Anda bisa meminta AI di Antigravity untuk membaca PRD ini sebelum mulai ngoding.

### A. Menulis User Stories
*User story* membantu kita memposisikan sistem dari sudut pandang pemakai aplikasi.
**Contoh untuk Aplikasi Villa:**
- *Sebagai **Guest** (Tamu), saya ingin mencari villa berdasarkan tanggal check-in, agar saya tahu properti mana yang *available* di tanggal liburan saya.*
- *Sebagai **Guest**, saya ingin melihat rincian fasilitas (Kamar tidur, Kolam renang, WiFi), agar saya yakin memesannya.*
- *Sebagai **Admin**, saya ingin melihat grafik transaksi bulan ini, agar saya tahu pendapatan bisnis.*

### B. Mengidentifikasi Entitas/Kebutuhan Data
Dari *User Stories* di atas, mari kita bedah tabel database apa saja yang dibutuhkan:
- Karena *Guest* harus login $\to$ Kita butuh tabel **`users`**.
- Karena ada katalog properti $\to$ Kita butuh tabel **`villas`**.
- Karena ada transaksi pesanan $\to$ Kita butuh tabel **`bookings`** dan **`payments`**.

Di sesi selanjutnya (Pertemuan 2), kita akan mensuplai/menyuapkan data PRD ini ke **Google Antigravity** untuk di-generate menjadi Database (Migration & Model Laravel) secara instan!
