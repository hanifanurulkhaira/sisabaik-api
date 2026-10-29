# Dokumentasi API - SisaBaik Chapter 8

Dokumentasi ini mencakup spesifikasi endpoint, metode HTTP, format request, serta struktur response dari SisaBaik REST API yang dilengkapi dengan sistem autentikasi JWT dan otorisasi berbasis peran (RBAC)

Base URL: http://127.0.0.1:3000

## Format Respons
- Respons berhasil menggunakan format `success: true` dan `data`, serta `meta` untuk koleksi
- Respons gagal menggunakan `success: false` dan `error` dengan kode (`code`) serta pesan (`message`) yang jelas
- Endpoint terlindungi membutuhkan header otorisasi: `Authorization: Bearer <accessToken>`

## Daftar Endpoint

### 1. Health Check
- GET /api/health
  - Fungsi: Memeriksa status proses server dan akses database.
  - Status: 200 OK

### 2. Autentikasi & Akun (`/api/auth`)
- POST /api/auth/registrasi
  - Fungsi: Pendaftaran mandiri akun pembeli baru (tanpa field peran/ID bebas)
  - Body JSON: `namaLengkap`, `email`, `kataSandi` (min. 15 karakter, maks. 72 byte UTF-8)
  - Status: 201 Created (mengembalikan data akun pembeli tanpa hash/token)
- POST /api/auth/registrasi-penyedia
  - Fungsi: Pendaftaran mandiri akun penyedia beserta profil usaha dalam satu transaksi (status usaha otomatis *menunggu*)
  - Body JSON: `namaLengkap`, `email`, `kataSandi`, `namaUsaha`, `alamat`, `nomorTelepon`
  - Status: 201 Created
- POST /api/auth/login
  - Fungsi: Masuk ke sistem menggunakan email dan kata sandi
  - Body JSON: `email`, `kataSandi`
  - Status: 200 OK (mengembalikan `accessToken`, `tokenType: "Bearer"`, `expiresIn`, dan data `pengguna`)
- GET /api/auth/saya
  - Fungsi: Mengambil informasi identitas aman milik pengguna yang sedang terautentikasi
  - Akses: Login (memerlukan token Bearer)
  - Status: 200 OK
- POST /api/auth/logout
  - Fungsi: Mencabut seluruh sesi token aktif (menaikkan `versi_token` di database)
  - Akses: Login
  - Status: 204 No Content (tanpa body response)

### 3. Penawaran (`/api/penawaran`)
- GET /api/penawaran
  - Fungsi: Katalog publik penawaran aktif, stok positif, belum kedaluwarsa, dari penyedia aktif dan terverifikasi (mendukung query `q`, `kategori`, `page`, `limit`)
  - Status: 200 OK
- GET /api/penawaran/milik-saya
  - Fungsi: Menampilkan daftar dan metadata penawaran khusus milik penyedia yang sedang login lintas status
  - Akses: Penyedia terverifikasi
  - Status: 200 OK
- GET /api/penawaran/:id
  - Fungsi: Detail satu penawaran tersedia berdasarkan ID string
  - Status: 200 OK (atau 404 jika tidak terlihat/tidak tersedia)
- POST /api/penawaran
  - Fungsi: Membuat penawaran baru (penyediaId ditentukan otomatis oleh server dari relasi akun)
  - Body JSON: `nama`, `kategori`, `hargaNormal`, `hargaPenawaran`, `stok`, `satuan`, `status`, `berakhirPada`
  - Akses: Penyedia terverifikasi
  - Status: 201 Created dengan header Location
- PUT /api/penawaran/:id
  - Fungsi: Mengubah atribut editable penawaran milik sendiri (penyediaId tidak boleh diganti)
  - Akses: Penyedia terverifikasi dan pemilik asli
  - Status: 200 OK
- DELETE /api/penawaran/:id
  - Fungsi: Menghapus penawaran milik sendiri
  - Akses: Penyedia terverifikasi dan pemilik asli
  - Status: 204 No Content (ditolak dengan 409 jika memiliki riwayat pesanan)

### 4. Pesanan (`/api/pesanan`)
- GET /api/pesanan
  - Fungsi: Koleksi ringkasan pesanan terpaginasi sesuai cakupan peran (pembeli melihat pesanannya sendiri, penyedia melihat pesanan yang ditujukan ke profilnya)
  - Akses: Pembeli atau penyedia terverifikasi
  - Status: 200 OK
- GET /api/pesanan/:id
  - Fungsi: Header ringkasan dan rincian item pesanan privat
  - Akses: Pihak yang berhak (pembeli pemilik atau penyedia terkait)
  - Status: 200 OK (atau 404 jika bukan haknya)
- POST /api/pesanan
  - Fungsi: Membuat pesanan atomik menggunakan body JSON berisi `items` (pembeliId diperoleh otomatis dari server)
  - Aturan: Satu pesanan wajib dari satu penyedia, stok dikunci (`FOR UPDATE`), memvalidasi waktu kedaluwarsa, serta menyimpan *snapshot* harga
  - Akses: Pembeli
  - Status: 201 Created dengan header Location

## Kode Status Error
- 400 Bad Request: Validasi input, format JSON, atau field kontrak/parameter tidak sesuai (misal menyisipkan field identitas terlarang)
- 401 Unauthorized: Gagal login, token tidak ada, rusak (`TOKEN_INVALID`), kedaluwarsa (`TOKEN_EXPIRED`), atau sesi dicabut (`SESSION_REVOKED`)
- 403 Forbidden: Identitas valid tetapi peran tidak diizinkan (`ROLE_FORBIDDEN`) atau profil penyedia belum disetujui (`PROVIDER_NOT_VERIFIED`)
- 404 Not Found: Resource tidak ditemukan atau disembunyikan untuk menjaga isolasi data objek langsung (ID milik orang lain)
- 409 Conflict: Konflik state seperti stok kurang (`INSUFFICIENT_STOCK`), penawaran tidak tersedia, beda penyedia (*mixed provider*), konflik registrasi, atau penghapusan data berelasi (*reference conflict*)
- 413 Payload Too Large: Body melebihi batas (maks. 20 KB)
- 415 Unsupported Media Type: Content-Type bukan `application/json` pada request POST/PUT
- 429 Too Many Requests: Batas laju tercapai (terpisah untuk login `AUTH_RATE_LIMIT` dan registrasi `REGISTRATION_RATE_LIMIT`, disertai header `Retry-After`)
- 500 / 503 Internal Server Error: Kegagalan server internal atau basis data