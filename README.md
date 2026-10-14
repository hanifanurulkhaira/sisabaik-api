<i>Developed by Hanifa Nurul Khaira (23367027)</i>

# sisabaik-api

sisabaik-api adalah proyek backend REST API berkembang pada mata kuliah Pengembangan Web. Aplikasi ini menyediakan
layanan antarmuka backend untuk menghubungkan server Express dengan basis data PostgreSQL (`sisabaik_dev`) guna
memfasilitasi pengelolaan penawaran makanan berlebih dan transaksi pemesanan secara atomik, aman, dan terintegrasi.

## Capaian Chapter 7
- Mempertahankan skema dan relasi database SisaBaik dari Chapter 6 dengan mengganti akses array sementara ke PostgreSQL.
- Mengintegrasikan Express dan PostgreSQL menggunakan kueri terparameterisasi (`$1`, `$2`) untuk mencegah SQL Injection.
- Membangun endpoint CRUD penawaran lengkap dengan pencarian (`q`), filter (`kategori`, `status`, `tersedia`), serta 
  paginasi (`page`, `limit`).
- Menerapkan transaksi database atomik (`BEGIN`, `COMMIT`, `ROLLBACK`) pada pembuatan pesanan dengan penguncian sto
  (`FOR UPDATE`), aturan *single provider*, dan *snapshot* harga.
- Memvalidasi path parameter, query parameter, format JSON, nilai uang `NUMERIC(12,0)`, stok, status, dan waktu ISO 8601 
  berzona waktu.
- Memetakan kesalahan validasi, konflik bisnis, dan kesalahan database menjadi respon JSON yang konsisten.
- Menyesuaikan halaman penguji lokal (`public/index.html` dan `public/app.js`) dengan identitas pembeli berbasis email 
  dan tipe ID string.

## Endpoint & Halaman Penguji

- `/api/health`: pemeriksaan kesehatan proses server dan koneksi basis data.
- `/api/penawaran`: endpoint CRUD koleksi dan detail penawaran makanan.
- `/api/pesanan`: endpoint pembuatan pesanan atomik dan rincian pesanan.
- `public/index.html`: halaman penguji interaktif lokal berbasis Express.

## Menjalankan proyek

1. Pastikan basis data PostgreSQL `sisabaik_dev` dari Chapter 6 aktif dan file `.env` telah dikonfigurasi.
2. Jalankan perintah berikut di terminal:

npm install
npm run dev