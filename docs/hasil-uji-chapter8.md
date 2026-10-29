# Laporan Hasil Uji - Chapter 8

Nama: Hanifa Nurul Khaira
NIM: 23367027
Database: sisabaik_dev (PostgreSQL)

## Ringkasan Hasil Pengujian

- Health Check dan Katalog Publik (200): 
  - Server dan PostgreSQL terhubung normal
  - Katalog publik (`GET /api/penawaran`) hanya menampilkan penawaran aktif dengan stok positif, belum kedaluwarsa, dari penyedia aktif yang sudah terverifikasi
- Pengujian Autentikasi dan Registrasi (201 & 401): 
  - Pendaftaran mandiri pembeli dan penyedia berhasil (`201 Created`), di mana kata sandi di-*hash* menggunakan `bcryptjs` (faktor kerja 12) dan data sensitif seperti *hash* atau token tidak bocor ke dalam *response*
  - Skrip pengujian otomatis (`npm run auth:test`) sukses melewati **44 assertion** pengujian HTTP, peran, dan kepemilikan data
  - Skenario *login* gagal, token rusak, atau token tanpa otorisasi ditolak dengan benar (`401 Unauthorized`)
- Manajemen Sesi dan Pencabutan Token (`SESSION_REVOKED`): 
  - Pengujian *logout* berhasil menaikkan `versi_token` di database, sehingga token lama di berbagai tab atau sesi langsung ditolak dengan status `401 SESSION_REVOKED`
  - Token yang telah kedaluwarsa setelah 15 menit juga secara konsisten ditolak dengan status `401 TOKEN_EXPIRED`
- Otorisasi Berbasis Peran dan Isolasi Data (`403` & `404`): 
  - Penyedia yang baru mendaftar berstatus *menunggu* ditolak melakukan operasi bisnis dengan status `403 PROVIDER_NOT_VERIFIED` sebelum disetujui oleh operator CLI
  - Pembeli yang mencoba membuat penawaran atau penyedia yang mencoba membuat pesanan ditolak dengan status `403 ROLE_FORBIDDEN`
  - Percobaan akses langsung ke objek milik orang lain (misal Budi mengubah penawaran Rina, atau Andi membaca pesanan Siti) berhasil diisolasi dan ditolak dengan status `404 Not Found` guna mencegah kebocoran informasi
- Pembatas Laju (*Rate Limiting*): 
  - Percobaan *login* berulang yang gagal dibatasi oleh `batasAuth` dan mengembalikan status `429 Too Many Requests` dengan kode `AUTH_RATE_LIMIT` serta header `Retry-After`
  - Endpoint pendaftaran juga memiliki pembatas laju terpisah (`REGISTRATION_RATE_LIMIT`)
- Transaksi Pemesanan Atomik & Snapshot Harga: 
  - Pembuatan pesanan oleh pembeli yang terautentikasi berhasil (`201 Created`) dengan identitas pembeli ditarik langsung dari server, penguncian stok (`FOR UPDATE`), pengurangan stok, serta penyimpanan *snapshot* harga yang tetap aman dari perubahan harga normal di kemudian hari

## Kesimpulan Pengujian
Seluruh skenario pengujian autentikasi, otorisasi berbasis peran (RBAC), pencabutan sesi, isolasi kepemilikan data antarpengguna, serta uji otomatis (`npm run auth:test`) berhasil dijalankan dengan status **PASS** Mekanisme keamanan server-side dan integritas data transaksi berjalan sesuai kontrak sistem