# Pertemuan 6: Integrasi Payment Gateway (Midtrans)

*Fokus: Mengotomatisasi penerimaan tagihan secara instan menggunakan infrastruktur Midtrans, sehingga sistem dapat memperbarui status pesanan tanpa verifikasi manual.*

---

## 1. Pemahaman Alur Kerja Payment Gateway

Dalam sebuah aplikasi bisnis berskala modern, mendaftarkan dan memverifikasi mutasi bank satu per satu secara manual sangatlah tidak efisien. Di sinilah Payment Gateway seperti **Midtrans** mengambil peran. Layanan ini menjadi jembatan antara aplikasi kita dengan puluhan metode pembayaran (Bank Transfer/VA, QRIS, e-Wallet, Kartu Kredit).

### A. Persiapan Akun Sandbox (Lingkungan Uji Coba)
Sebelum peluncuran nyata, kita wajib menguji aliran dana menggunakan simulasi (*Sandbox*).
- **Prosedur Dasar:** Mendaftar akun pengembang Midtrans dan mencari tiga kredensial utama di dasbor: *Merchant ID*, *Client Key*, dan *Server Key*.
- **Konfigurasi Lokal:** Memasukkan kredensial rahasia tersebut ke dalam fail variabel lingkungan (`.env`) Laravel agar kode sistem bisa terhubung dengan jembatan Midtrans secara aman.
- **Instalasi Pustaka (Library):** Meminta IDE menjalankan perintah `composer require midtrans/midtrans-php` di terminal sebagai kerangka komunikasi (SDK) resmi Midtrans.

---

## 2. Pembuatan Tagihan Snap (Snap Token Generation)

Midtrans menyediakan layanan visual bawaan bernama **Snap Widget**. Fitur ini memungkinkan pengguna melihat bermacam-macam opsi bank pembayaran langsung berbentuk layar *pop-up* tanpa harus melakukan perpindahan ke situs pihak ketiga.

### A. Persiapan Backend
Segera setelah pengguna mengklik "Bayar Sekarang", sistem wajib meminta izin *Snap Token* kepada Midtrans.

**Implementasi Cerdas dengan Prompt:**
> *"Di dalam `PaymentController`, buat method `getSnapToken`. Tambahkan library `\Midtrans\Snap` dan konfigurasi `\Midtrans\Config::$serverKey = env('MIDTRANS_SERVER_KEY')`. Siapkan Array `params` yang berisi: algoritma 'transaction_details' (`order_id` berisi id tabel `bookings`, `gross_amount` berisi total akhir harga), serta rincian pembeli di dalam 'customer_details'. Kembalikan keluaran `Snap::getSnapToken($params)` tersebut berbentuk token ke halaman antarmuka tagihan pemesan."*

### B. Persiapan Frontend
- Di file *Blade* Laravel, Anda menyematkan *script* Midtrans Javascript SDK.
- Tombol aksi HTML dirangkaikan untuk mengeksekusi layanan `window.snap.pay("TOKEN_HERE")`, yang kemudian memicu jendela pembayaran fiksasi virtual untuk diselesaikan pengguna.

---

## 3. Komunikasi Dua Arah Eksternal (Webhook Callback)

Tahap terpenting dan tersulit dalam alur pembayaran adalah memastikan bahwa dana yang masuk ke rekening virtual terekam secara independen tanpa campur tangan staf admin. Proses ini diistilahkan **Notification Handler** atau **Webhook**.

### A. Mekanisme Notifikasi Background (Asinkron)
Ketika sistem bank (contoh: BCA Virtual Account) memproses transfer Anda dengan sukses, server Midtrans akan "mengetuk pintu" / mengirim sinyal *POST* secara diam-diam ke alamat `URL/api/midtrans-callback` di proyek Anda. 

- **Pembebasan Keamanan Lintas-Situs (CSRF Exemption):** Hal pertama adalah mengecualikan direktori `/api/` tersebut dari proteksi dasar CSRF agar sinyal dari server eksternal Midtrans dapat masuk tanpa tertolak.

### B. Lapisan Validasi Kriptografi (Anti-Spoofing)
Risiko tertinggi webhook adalah seorang peretas berpura-pura menjadi server Midtrans (Spoofing) dengan mengirim permintaan POST palsu bertuliskan `"Status Order = PAID"`. 

**Skenario Proteksi Cerdas Vibe Coding:**
> *"Buat metode `notificationHandler` di `PaymentController`. Tangkap data masuk berbentuk JSON POST dari webhook Midtrans. Bentuk satu lapis sistem verifikasi kunci Enkripsi SHA512 berdasarkan rumusan `order_id` + `status_code` + `gross_amount` + `Server_Key`. Tarik perbandingan (komparasi kueri); apabila hasil Hash Enkripsi gagal cocok (*mismatch*), tolak dengan kode respon http 403. Jika otentikasi identik dan status bertransisi `settlement` (Lunas), barulah eksekusi instruksi pembaruan (update query) pada tabel `bookings` menjadi status 'paid'."*

Keberhasilan di sesi ini secara utuh melepaskan beban tenaga perantara manusia, membiarkan aplikasi Anda menerima laba tunai sembari terus membukukan catatan secara presisi selama 24 jam nonstop.
