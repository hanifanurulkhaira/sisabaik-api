<i>Developed by Hanifa Nurul Khaira (23367027)</i>

# SisaBaik - Integrasi Full Stack Vue & Express API (Chapter 10)

Proyek aplikasi web full stack SisaBaik yang mengintegrasikan antarmuka frontend berbasis Vue 3 + Vite dengan server backend REST API berbasis Node.js/Express dan PostgreSQL. Modul ini menggantikan data tiruan di memori dengan komunikasi API persisten, autentikasi berbasis peran, manajemen unggah foto penawaran, serta alur transaksi pesanan dan pemulihan stok secara menyeluruh.

## Capaian Modul 10 (Integrasi Full Stack)
- Konfigurasi Proxy & Jalur Lintas Origin: Menghubungkan frontend Vite ke backend Express melalui proxy `/api` pada satu host konsisten guna menghindari kendala CORS.
- Autentikasi & Otorisasi Peran: Mengimplementasikan alur pendaftaran dan login terpisah untuk peran Pembeli dan Penyedia Usaha, lengkap dengan penjagaan rute halaman (*route guard*) dan kontrol akses API berdasarkan peran.
- Aturan Peninjauan Usaha: Akun penyedia usaha baru berstatus menunggu peninjauan dan wajib diverifikasi oleh pengelola sebelum dapat memublikasikan penawaran aktif ke katalog publik.
- Manajemen Penawaran & Payload Foto/Versi/Aksi: Penyedia dapat membuat dan memperbarui penawaran (harga normal, harga penawaran, stok, satuan, waktu kedaluwarsa) menggunakan payload terstruktur, mendukung unggahan aset gambar (PNG/JPEG/WebP) dengan mempertahankan foto lama saat pembaruan, serta fallback ilustrasi kategori otomatis.
- Reservasi, Pembayaran di Tempat & Pemulihan Stok: Pembeli dapat membuat pesanan yang memotong stok secara otomatis di sisi server menggunakan snapshot harga server. Pembatalan sebelum konfirmasi maupun penolakan oleh penyedia mengembalikan stok sekali secara aman. Penyelesaian transaksi mendukung skema pembayaran tunai di tempat maupun pesanan gratis tanpa transaksi uang.
- Definisi Metrik Dasbor: Menghitung akumulasi belanja/pendapatan selesai, potensi/aktual penghematan (selisih nilai normal dan nilai transaksi), serta total unit tersalurkan berdasarkan perubahan dari baseline transaksi yang berstatus selesai.

## Struktur Direktori Proyek
- `/database`: Berisi skrip migrasi dan skema tabel relasional transaksi (`detail_pesanan`, `penawaran`, `pesanan`).
- `/frontend/src/views`: Berisi halaman antarmuka terintegrasi (`KatalogView.vue`, `LoginView.vue`, `RegistrasiView.vue`, `PenyediaView.vue`, `ProfilPenyediaView.vue`, `PesananView.vue`, `DetailPesananView.vue`, `DashboardView.vue`, `InfoView.vue`).
- `/frontend/src/components`: Berisi komponen modular Vue (`FilterPenawaran.vue`, `FormPenawaran.vue`, `KartuPenawaran.vue`, `Ikon.vue`).
- `/frontend/src/router`: Berisi konfigurasi rute aplikasi serta penjagaan akses halaman berdasarkan sesi dan peran pengguna.
- `/frontend/src/lib`: Berisi modul manajemen sesi autentikasi (`sesi.js`), pengiriman form penawaran (`form-penawaran.js`), dan helper tampilan/visual.
- `/frontend/src/assets`: Berisi aset font biner (`PlusJakartaSans`), ikon SVG, dan ilustrasi kategori makanan.

## Lingkungan & Menjalankan Aplikasi Lokal

Pastikan lingkungan pengembangan menggunakan versi Node.js LTS terbaru dan database PostgreSQL sudah aktif. Gunakan dua terminal terpisah untuk menjalankan backend dan frontend secara bersamaan:

1. Menjalankan Server Backend (Root `sisabaik-api` - Port 3000):
```bash
npm install
npm run dev

2. Menjalankan Server Frontend (Direktori frontend - Port 5173):
```bash
cd frontend
npm install
npm run dev

3. Build Produksi & Pratinjau Lokal (Direktori frontend):
```bash
npm run build
npm run preview