<i>Developed by Hanifa Nurul Khaira (23367027)</i>

# sisabaik-api

sisabaik-api adalah proyek backend REST API berkembang pada mata kuliah Pengembangan Web. Aplikasi ini menyediakan layanan antarmuka backend untuk menghubungkan server Express dengan basis data PostgreSQL (`sisabaik_dev`) guna memfasilitasi pengelolaan autentikasi, otorisasi berbasis peran (RBAC), penawaran makanan berlebih, serta transaksi pemesanan secara aman, atomik, dan terintegrasi.

## Capaian Chapter 8
- Menerapkan migrasi database melalui skrip `database/07_auth.sql` untuk menambahkan kolom `kata_sandi_hash` dan `versi_token` pada tabel pengguna tanpa merusak data historis.
- Mengimplementasikan pendaftaran mandiri untuk pembeli dan penyedia dengan enkripsi kata sandi serta status verifikasi usaha untuk penyedia.
- Menerbitkan dan memverifikasi token JWT 
- Melindungi *route* menggunakan *middleware* (`wajibLogin`, `wajibPeran`, dan `wajibPenyediaTerverifikasi`) serta mengambil identitas dari pengguna yang terautentikasi
- Membatasi mutasi penawaran hanya pada penyedia pemilik yang terverifikasi serta akses pesanan pada pihak yang berhak (isolasi data antar penyedia dan pembeli)
- Menerapkan pencabutan token secara global melalui mekanisme *logout* yang menaikkan `versi_token` di database (`401 SESSION_REVOKED`)
- Menambahkan pembatas laju (*rate limiting*) menggunakan `express-rate-limit` untuk mengamankan endpoint login dan pendaftaran dari serangan *brute-force*.
- Menyediakan halaman penguji interaktif (`public/auth.html`) serta skrip pengujian otomatis (`npm run auth:test`) untuk memvalidasi seluruh skenario positif dan negatif.

## Endpoint & Halaman Penguji
- `/api/health`: Pemeriksaan kesehatan proses server dan koneksi basis data.
- `/api/auth`: Endpoint pendaftaran pembeli/penyedia, login, pemeriksaan identitas, dan pencabutan sesi .
- `/api/penawaran`: Endpoint CRUD koleksi dan detail penawaran makanan dengan pembatasan hak akses pemilik
- `/api/pesanan`: Endpoint pembuatan pesanan atomik berbasis akun terautentikasi dan rincian pesanan privat
- `public/auth.html`: Halaman penguji interaktif lokal berbasis Express untuk manajemen akun dan otorisasi

## Menjalankan Proyek
1. Pastikan basis data PostgreSQL `sisabaik_dev` tersedia dan file `.env` telah dikonfigurasi dengan `JWT_SECRET` berukuran 64 digit hex
2. Jalankan perintah berikut di terminal:
```bash
npm install
npm run dev