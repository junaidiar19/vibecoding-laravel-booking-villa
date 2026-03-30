# Silabus Masterclass: Membangun Aplikasi Booking Villa dengan Laravel (Pendekatan "Vibe Coding")

**Durasi:** 8x Pertemuan  
**Target Output:** Aplikasi Booking Villa MVP dengan integrasi Live Chat/Chatbot, Payment Gateway, Test Coverage, hingga tahapan Deployment.  
**Metodologi:** *Vibe Coding* (Menggunakan **Google Antigravity** sebagai IDE pintar untuk mempercepat penulisan kode).

---

## Pertemuan 1: Fundamental "Vibe Coding" & Perencanaan Proyek (PRD)
*Fokus: Memahami mindset developer modern dan merumuskan spesifikasi aplikasi.*

- **Konsep "Vibe Coding":**
  - Pergeseran peran programmer dari pengetik kode manual menjadi "Arsitek Sistem" & "Reviewer".
  - Pengenalan **Google Antigravity**: Menggunakan IDE Cerdas ini sebagai asisten *pair-programming*.
  - Teknik *Prompting* yang efektif untuk konteks pengembangan web dengan Laravel.
- **Produk Inception:**
  - Studi kasus: Aplikasi Booking Villa.
  - Membedah fitur esensial MVP: Katalog Villa, Autentikasi User, Flow Booking, Checkout, Integrasi Pembayaran.
- **Membuat PRD (Product Requirements Document):**
  - Menyusun *User Stories* & Kebutuhan Data (Entitas) secara sistematis.

## Pertemuan 2: Desain Database & Setup Proyek
*Fokus: Meletakkan pondasi data yang solid.*

- **Arsitektur Database (ERD):**
  - Merancang skema tabel: `users`, `villas`, `bookings`, `payments`.
  - Membangun relasi *One-to-Many* (Contoh: User dengan Booking).
- **Eksekusi dengan "Vibe Coding" (Antigravity):**
  - Menginstruksikan Antigravity untuk *scaffolding* Laravel versi terbaru.
  - Men-generate *Migrations*, *Models*, *Factories*, dan *Seeders* dengan efisien.
  - Best practice *Naming Conventions* di Laravel agar konsisten dan mudah dipahami oleh AI di masa mendatang.

## Pertemuan 3: Integrasi Core Feature & Autentikasi
*Fokus: Membangun pondasi interaksi dasar pada aplikasi.*

- **Setup Otoritas & Keamanan Akses Dasar:**
  - Implementasi register/login instan (menggunakan Laravel Breeze).
  - Melindungi *route* yang sensitif dengan *Middleware* (Hanya user login yang bisa mem-booking).
- **CRUD (Create, Read, Update, Delete) Data Utama:**
  - *Seeding* data tiruan (dummy data) menggunakan *Faker* untuk villa.
  - Membangun halaman katalog villa dan halaman detail masing-masing properti.

## Pertemuan 4: Algoritma Pencarian & Alur Booking Utama 
*Fokus: Merancang logika utama aplikasi pencarian dan pesanan.*

- **Sistem Pencarian Villa (Standard Search):**
  - Query database *Eloquent* berbasis parameter (Tipe Villa, Lokasi, Kapasitas) secara aman.
- **Flow Checkout & Pembuatan Booking:**
  - Siklus data transaksi: dari klik "Pesan", kalkulasi durasi menginap terhadap harga per malam.
  - Merancang manajemen *status* (`pending`, `paid`, `canceled`).
  - Menyimpan *record* ke dalam tabel `bookings` sebagai entri *pending payment*.

## Pertemuan 5: Pengamanan Bisnis Logic (Security & Validasi)
*Fokus: Melindungi aplikasi dari celah kerentanan fatal ("Hacking" atau eksploitasi logika).*

- **Dasar Security Vulnerability:**
  - Mengenal kembali CSRF, XSS, dan proteksi *SQL Injection* via PDO.
- **Data Tampering Protection (Keamanan Harga):**
  - Mencegah *user* memanipulasi kode *front-end* (contoh: Mengganti total `harga: Rp. 2.500.000` menjadi `Rp. 1` di HTML Inpsect Element).
  - Validasi ulang di sisi Backend/Server.
- **Mencegah "Race Condition" atau Double Booking:**
  - Cara mencegah satu villa dipesan oleh dua orang berbeda di tanggal yang sama secara bersamaan (Database Locking & Validation checking).

## Pertemuan 6: Integrasi Payment Gateway (Midtrans)
*Fokus: Mengotomatisasi proses pembayaran layaknya e-commerce sungguhan.*

- **Konsep Payment Gateway & Alur Kerja (Midtrans):**
  - Pembuatan akun Sandbox dan konfigurasi API Key ke `.env`.
- **Integrasi Midtrans Snap:**
  - Men-generate `Snap Token` di backend saat *checkout* sukses.
  - Menampilkan Pop-Up Pembayaran *Payment Widget* kepada user.
- **Callback / Webhook:**
  - Merancang *endpoint* API penerima notifikasi otomatis (misal: user sukses bayar via bank transfer).
  - **Sangat Krusial:** Validasi *Signature Key* untuk menangkal notifikasi palsu (Spoofing).

## Pertemuan 7: Integrasi 3rd-Party Chatbot & Automated Testing
*Fokus: Menambah interaktivitas (Live Chat) dan memastikan kualitas kode stabil.*

- **Menambahkan Fitur "AI Assistant / Chatbot" (Pihak Ke-3):**
  - Mengintegrasikan widget Live Chat populer seperti **tawk.to**.
  - Embedding *script* Tawk.To ke dalam *layout* utama Blade Laravel.
- **Automated Testing (PestPHP):**
  - Kenapa menulis "Test" krusial di era AI? (Sebagai *safeguard* bila ide salah generate logika).
  - *Unit Test*: Mengetes kalkulasi murni (harga * durasi menginap = total).
  - *Feature Test*: Simulasi API memukul endpoint checkout (*Mocking Endpoint*).

## Pertemuan 8: Arsitektur Deployment & Cloud Server
*Fokus: Membawa aplikasi lokal agar bisa diakses seluruh dunia.*

- **Opsi Hosting Tradisional (Shared Hosting - cPanel):**
  - Limitasi dan cara *deploy* termurah/tradisional dengan FTP & *symlink*.
- **Opsi Modern (VPS - Virtual Private Server):**
  - Apa itu VPS (DigitalOcean / AWS / Vultr).
  - Overview *Provisioning* server *raw* (Linux dasar, Nginx, PHP-FPM, MySQL).
- **Opsi Praktis Berbayar (Laravel Forge):**
  - Fitur inti **Laravel Forge**: Menghubungkan Git Repository ke VPS secara *seamless*.
  - Push-to-deploy *(Zero Downtime Deployment)* dan pengelolaan SSL Cepat (LetsEncrypt).
