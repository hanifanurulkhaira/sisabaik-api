# Dokumentasi API - SisaBaik Chapter 7

Dokumentasi ini mencakup spesifikasi endpoint, metode HTTP, format request, dan struktur response dari SisaBaik REST API.

Base URL: http://127.0.0.1:3000/api

## Format Respons
- Respons berhasil menggunakan format success true dan data, serta meta untuk koleksi.
- Respons gagal menggunakan success false dan error dengan kode serta pesan yang jelas.

## Daftar Endpoint

### 1. Health Check
- GET /api/health
  - Fungsi: Memeriksa status proses server dan akses database.
  - Status: 200 OK

### 2. Penawaran
- GET /api/penawaran
  - Fungsi: Koleksi penawaran dengan query q, kategori, status, tersedia, page, dan limit.
  - Status: 200 OK
- GET /api/penawaran/:id
  - Fungsi: Detail satu penawaran berdasarkan ID string.
  - Status: 200 OK
- POST /api/penawaran
  - Fungsi: Membuat penawaran baru dengan body JSON (penyediaId, nama, kategori, hargaNormal, hargaPenawaran, stok, satuan, status, berakhirPada).
  - Status: 201 Created dengan header Location
- PUT /api/penawaran/:id
  - Fungsi: Mengganti seluruh atribut editable penawaran (penyediaId tidak boleh diubah).
  - Status: 200 OK
- DELETE /api/penawaran/:id
  - Fungsi: Menghapus penawaran yang belum memiliki riwayat pesanan.
  - Status: 204 No Content (ditolak dengan 409 jika berelasi)

### 3. Pesanan
- GET /api/pesanan
  - Fungsi: Koleksi ringkasan pesanan terpaginasi.
  - Status: 200 OK
- GET /api/pesanan/:id
  - Fungsi: Header dan rincian item pesanan.
  - Status: 200 OK
- POST /api/pesanan
  - Fungsi: Membuat pesanan atomik menggunakan body JSON (emailPembeli dan items).
  - Aturan: Satu pesanan wajib dari satu penyedia, stok dikunci (FOR UPDATE), dan menyimpan snapshot harga.
  - Status: 201 Created dengan header Location

## Kode Status Error
- 400 Bad Request: Validasi input, format JSON, atau constraint data salah.
- 404 Not Found: Resource atau pembeli tidak ditemukan.
- 409 Conflict: Konflik state seperti stok kurang, beda penyedia, atau delete berelasi.
- 413 Payload Too Large: Body melebihi 20 KB.
- 415 Unsupported Media Type: Content-Type bukan application/json.
- 500 Internal Server Error: Kesalahan server tak terduga.
- 503 Service Unavailable: Database belum dapat diakses.