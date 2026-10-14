# Laporan Hasil Uji - Chapter 7

Nama: Hanifa Nurul Khaira
NIM: 23367027
Database: sisabaik_dev (PostgreSQL)

## Ringkasan Hasil Pengujian

- Health Check dan GET Koleksi (200): Berhasil terhubung ke server dan PostgreSQL, data tampil dengan envelope dan meta 
  yang benar.
- Validasi Input dan Parameter (400): Pengujian ID tidak valid, format JSON rusak, Content-Type salah, payload di atas 
  20 KB, dan field tidak dikenal berhasil ditolak dengan kode error yang sesuai tanpa merusak database.
- CRUD Penawaran:
  - POST berhasil membuat data baru (201) dan mengembalikan header Location.
  - PUT berhasil memperbarui data secara utuh dengan aturan immutable provider.
  - DELETE berhasil menghapus data tanpa riwayat (204) dan otomatis menolak penghapusan penawaran yang sudah berelasi 
    dengan riwayat pesanan (409).
- Pencarian, Filter, dan Paginasi: Filter gabungan (q, kategori, status, tersedia) dan paginasi berjalan akurat; query 
  teks aman dari injeksi.
- Transaksi Pemesanan Atomik:
  - Pesanan valid berhasil dibuat (201) dengan penguncian stok (FOR UPDATE), pengurangan stok otomatis, dan penyimpanan 
    snapshot harga.
  - Skenario negatif seperti pesanan lintas penyedia (MIXED_PROVIDER) dan stok kurang (INSUFFICIENT_STOCK) berhasil 
    ditolak (409) tanpa mutasi sebagian melalui rollback.
- Konkurensi Stok dan Persistensi:
  - Uji dua request bersamaan pada penawaran berstok satu menghasilkan tepat satu sukses (201) dan satu ditolak (409) 
    dengan stok akhir 0.
  - Data pesanan dan snapshot harga terbukti tetap aman dan persisten setelah server di-restart manual.

## Kesimpulan Pengujian
Seluruh skenario pengujian dasar (npm run api:test) dan uji konkurensi stok (node scripts/uji-konkurensi.js) berhasil 
dijalankan. Persistensi data, snapshot harga, dan penguncian stok berjalan sesuai kontrak transaksi PostgreSQL.