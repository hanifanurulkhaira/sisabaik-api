# Laporan Hasil Uji - Chapter 10 (Integrasi Full Stack Vue & Express API)

Nama: Hanifa Nurul Khaira
NIM: 23367027

## 1. Ringkasan Eksekusi & Pengujian
Pengujian end-to-end  dilakukan secara lokal dengan mengintegrasikan antarmuka frontend Vue 3 + Vite (Port 5173) dengan server backend Node.js/Express dan database relasional PostgreSQL (Port 3000). Seluruh alur data beralih dari memori lokal fixture ke basis data persisten dengan menerapkan autentikasi berbasis peran, manajemen unggah berkas, validasi payload, dan pemulihan stok transaksional.

## 2. Aliran Autentikasi & Otorisasi Peran
- `RegistrasiView.vue` & `LoginView.vue`: Mengelola pendaftaran dan login terpisah untuk peran Pembeli dan Penyedia Usaha, mengirimkan kredensial aman ke backend melalui proxy `/api`.
- `PenyediaProfilView.vue` & `PenyediaView.vue`: Mengelola status akun penyedia yang memerlukan verifikasi/peninjauan oleh pengelola sebelum dapat menerbitkan penawaran aktif.
- `Router Guard`: Menjaga akses rute privat (seperti manajemen pesanan dan dasbor) agar tidak dapat diakses tanpa sesi yang valid atau oleh peran yang tidak sesuai.

## 3. Alur Transaksi, Reservasi & Pemulihan Stok
- Pembuatan Pesanan: Pembeli memilih penawaran dan membuat pesanan; server secara aman memotong stok relasional, mencatat snapshot harga, dan mengembalikan status `201 Created`.
- Pembatalan & Penolakan: Pembatalan pesanan oleh pembeli sebelum konfirmasi atau penolakan oleh penyedia memicu pemulihan stok relasional secara otomatis sebanyak sekali.
- Penyelesaian Transaksi: Alur berlanjut dari menunggu konfirmasi, siap diambil, hingga selesai melalui skema pembayaran tunai di tempat atau pesanan gratis tanpa transaksi moneter.

## 4. Hasil Skenario Pengujian 
1. Akses Tamu & Pembatasan Route: Tamu dapat membaca katalog publik, namun akses ke route privat (`/#/pesanan`) secara otomatis diarahkan ke halaman login.
2. Registrasi & Verifikasi Penyedia: Akun penyedia baru berhasil terdaftar dengan status menunggu peninjauan, dan dapat mempublikasikan tiga penawaran uji setelah diverifikasi.
3. Manajemen Penawaran & Payload: Penyedia berhasil membuat dan menyunting penawaran (Paket Roti, Nasi Hemat, Sayur Gratis) dengan validasi harga, stok, unggahan foto (PNG/JPEG/WebP), serta ilustrasi fallback kategori.
4. Transaksi & Validasi Stok Pembeli: Pembuatan pesanan dengan kuantitas 2 berhasil memotong stok server dengan tepat, mencatat snapshot harga, serta menampilkan detail subtotal yang akurat.
5. Keamanan & Isolasi Akses: Akses detail pesanan asing oleh pengguna lain menghasilkan galat `404`, dan akses rute penyedia oleh pembeli memunculkan penolakan akses.
6. Metrik Dasbor & Penyelesaian: Proses serah terima pesanan berbayar maupun gratis berhasil memperbarui akumulasi belanja/pendapatan, unit tersalurkan, serta metrik penghematan pada dasbor masing-masing peran.

## 5. Lampiran Dokumentasi Hasil Uji & Terminal
- Tampilan Autentikasi & Registrasi: Berhasil menguji halaman pendaftaran dan login akun pembeli serta penyedia usaha dengan penanganan respons API backend yang akurat.
- Tampilan Status Peninjauan Usaha: Berhasil memverifikasi halaman profil usaha penyedia yang berada dalam status menunggu peninjauan sebelum diaktifkan oleh pengelola.
- Tampilan Pengelolaan Penawaran: Berhasil menguji pembuatan tiga penawaran aktif (berbayar dengan unggah foto dan tanpa foto dengan ilustrasi kategori) melalui antarmuka penyedia.
- Tampilan Transaksi & Detail Pesanan: Berhasil memproses pesanan pembeli, mencatat snapshot harga server, serta menguji alur pembatalan dengan pemulihan stok.
- Tampilan Konfirmasi & Pengambilan Pesanan: Berhasil menguji alur penyedia mulai dari konfirmasi pesanan, penandaan siap diambil, hingga pencatatan pembayaran di tempat dan serah terima.
- Tampilan Metrik Dasbor Akhir: Berhasil memverifikasi pembaruan data metrik belanja, pendapatan, diskon, dan unit tersalurkan pada dasbor setelah transaksi berstatus selesai.
- Hasil Proses Terminal Backend & Frontend: Berhasil menjalankan server backend Express di port 3000 dan server frontend Vite di port 5173 secara bersamaan tanpa kendala koneksi proxy `ECONNRESET`