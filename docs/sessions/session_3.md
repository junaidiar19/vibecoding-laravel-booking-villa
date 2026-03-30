# Pertemuan 3: Integrasi Core Feature & Autentikasi

*Fokus: Melakukan proses seeding pembuatan data pengujian dan merancang antarmuka daftar properti serta fitur pembatasan wilayah akses (autentikasi).*

---

## 1. Setup Autentikasi dan Proteksi Hak Akses

Pada sebuah aplikasi booking, interaksi sangat bergantung pada identitas pengguna (siapa yang mem-booking). 

### A. Konfigurasi Awal Laravel Breeze
Pada sesi sebelumnya instalasi proyek dilakukan bersama parameter `--breeze`. Laravel Breeze memberikan fondasi login dan register *out-of-the-box* yang aman dan terstandardisasi. 
- Meninjau *routes* bawaan Breeze di `routes/auth.php`.
- Memeriksa file tampilan yang sudah di-*generate* otomatis di folder `resources/views/auth/`.

### B. Menerapkan Proteksi Middleware 'auth'
Aplikasi harus mampu mengelola diferensiasi hak akses publik dan terotorisasi:
- **Hak Akses Publik:** Semua pengunjung (meskipun belum login) berhak melihat katalog villa.
- **Hak Akses Privat:** Tombol "Pesan Sekarang" dan form konfirmasi hanya terbuka apabila sesi login aktif terdeteksi.
- **Implementasi (Vibe Coding):**
  Anda cukup menginstruksikan IDE: *"Terapkan middleware auth pada seluruh route controller Booking (kecuali fungsi public seperti list/show), dan alihkan pengunjung non-login secara otomatis kembali ke halaman rute `/login`."*

---

## 2. Generate Data Pengujian (Seeder & Faker)

Aplikasi katalog yang kekurangan *database content* akan menyulitkan pengujian penataan *layout grid* di fase *frontend*. Penanganan tercepat adalah menugaskan AI untuk men-generate data pengujian.

### A. Merancang Data Pabrikan (Factory)
Kita wajib memberikan batasan yang terstruktur. Tuliskan instruksi pada prompt Google Antigravity layaknya:
> *"Di dalam `VillaFactory`, buatkan definisi seeder menggunakan perpustakaan sistem Faker. Generate nama properti (contoh: 'Villa Cempaka Bali'), atur harga per malam direntang 500ribu hingga 3juta rupiah, kapasitas ruangan statis diantara 2 sampai 10 orang, serta berikan placeholder URL gambar acak dari unsplash."*

### B. Menjalankan Database Seeder
Setelah *Factory* dan *Seeder* dikompilasi, kita jalankan perintah Terminal `php artisan db:seed`. Proses ini mengisi relasi tabel dengan total 50 baris properti siap pakai dari hasil simulasi *factory*.

---

## 3. Menampilkan Halaman Katalog (Read)

Fitur fungsional awal fokus dalam mempresentasikan properti selengkap mungkin kepada calon peminat villa.

### A. Routing dan Controller
- **Route Setup:** Mendaftarkan jalur `GET /villas` yang mengarah ke pemanggilan fungsi `index` milik `VillaController`.
- **Logika Pagination:** Menginstruksikan AI untuk menggunakan teknik `->paginate()` untuk menghindari pengambilan *load* berlebihan jika data berkembang hingga ribuan kueri baris sekaligus.

### B. Desain Antarmuka Terintegrasi Blade
- **Komponen Tampilan UI:** Menyusun layout grid *Tailwind CSS* menampilkan detail visual (Thumbnail, Harga, Kapasitas) atas data *eloquent collection* villa.
- **Halaman Rincian (*Show Route*):** Mempraktikkan proses URI dinamis `/villas/{id}` demi menampilkan deskripsi lebih dalam atas sebuah rekaman database.
- **Vibe Coding Flow (View Integrasi):** Dengan kapabilitas Antigravity, Anda direkomendasikan memberi perintah: *"Buka dan baca file html_template_beranda.html, aplikasikan blok HTML tersebut ke dalam struktur index.blade.php secara modular untuk looping koleksi $villas."*
