# Pertemuan 8: Arsitektur Deployment & Cloud Server

*Fokus: Mengudara (Going Live). Menyiapkan infrastruktur peladen guna memublikasikan aplikasi ke internet agar dapat diakses oleh publik.*

---

## 1. Opsi Konvensional: Shared Hosting (cPanel)

Lingkungan *Virtual Hosting* atau *Shared Hosting* adalah gerbang paling terjangkau bagi pemula untuk merilis aplikasi mereka. 

### A. Keterbatasan dan Profil Penggunaan
Opsi ini cocok sekadar untuk uji coba portofolio berskala kecil karena performa *RAM/CPU* dipakainya berbagi dengan ratusan pengguna (situs) lain. Pengembang sering tidak diberikan hak akses *Root Terminal (SSH)* penuh.

### B. Tantangan Penataan Direktori (Storage Symlink)
- **Komplikasi Struktur:** Folder publik aplikasi Laravel (direktori `/public`) memiliki sifat bertolak belakang dengan bawaan *root* cPanel (yakni `public_html`).
- **Strategi Publikasi FTP:** Anda diwajibkan mengemas proyek ke dalam fail arsip (ZIP), mengunggahnya melalu protokol transfer berkas (FTP) / *File Manager* dasbor *cPanel*, dan mengekstraknya di jalur di luar `public_html`.
- **Manajemen Aset Gambar:** Mengajarkan cara merakit kaitan simbolik (*Symlink*) manual—atau memodifikasi fail `index.php`—agar peramban pengunjung dapat memuat pustaka gambar antarmuka secara sempurna tanpa risiko mengarahkan akses rahasia fail `.env`.

---

## 2. Opsi Skala Industri: Virtual Private Server (VPS)

Apabila volume pengunjung harian melonjak, fungsionalitas pengiriman surel faktur instan diaktifkan, atau beban antrean basis data memberat, pengembang wajib bermigrasi ke wadah *VPS* murni layaknya *DigitalOcean, AWS, Linode,* atau *Vultr*.

### A. Hak Spesifik (Root Access) Kendali Mutlak
Pada *VPS*, komputasi yang ditebus sifatnya sangat eksklusif (terisolir). Lingkungannya sama halnya dengan menyewa unit komputer utuh.

### B. Pembangunan Infrastruktur Kosong (Server Provisioning)
Pengembang wajib mengetahui pilar tumpukan piranti lunak *(Software Stack)* apa saja yang harus terpasang untuk melayani *backend* framework Laravel:
- **Web Server:** Mengompilasi dan mengamankan peladen **Nginx**.
- **Bahasa & Pemrosesan:** Menyinkronkan fungsi modul **PHP-FPM** dan ekstensi pelengkapnya (termasuk *Composer*).
- **Basis Data:** Memasang **MySQL / PostgreSQL** untuk komparasi tabel operasional pesanan.

---

## 3. Otomatisasi Profesional: Rilis CI/CD via Laravel Forge

Membangun fondasi Nginx, mengatur otorisasi pengguna Linux (Ubuntu), hingga menambal celah pembaruan berkala amatlah menghabiskan kapabilitas energi *Software Engineer*. Oleh karena itu, *Creator* dari Laravel menciptakan platform eksternal pendamping tangguh bernama **Laravel Forge**.

### A. Keunggulan Orkestrasi Otomatis (Seamless Control)
Laravel Forge akan "membajak" dan mengambil alih tata kelola baris perintah (Terminal) *VPS* kosongan Anda secara jarak jauh, lantas mengonfigurasi segalanya sesuai pedoman keamanan terbaik *(best-practice)* tanpa Anda mengetik satu baris komando Linux.

### B. Menerapkan "Push-to-Deploy" (Zero Downtime)
- Forge akan menyambungkan (autentikasi kunci) antara repositori **GitHub** ke peladen **VPS** Anda.
- Manakala baris *Vibe Coding* direvisi lokal milik Anda dan di *Push* kembali ke *Branch Main* GitHub, Forge akan menyadari komit baru tersebut secara seketika (*Webhooks*).
- Platform ini meluncurkan *bash script* terintegrasi: `composer install --no-dev`, pembersihan struktur tembolok (*route cache*), `php artisan migrate --force`, hingga penyusunan bundel *NPM Frontend* di latar belakang sistem tanpa memutuskan trafik aplikasi sedetik pun. 

Ditutup dengan fitur penyematan Enkripsi Gembok URL HTTPS otomatis *(SSL Certificate)* dari Let's Encrypt dalam satu tombol kursor. 

Dengan peresmian materi penyebaran aplikasi di Pertemuan 8 ini, tuntas sudah pembekalan terpusat (Masterclass) keilmuan pembuatan produk perangkat lunak modifikasi arsitektur Laravel Vibe Coding!
