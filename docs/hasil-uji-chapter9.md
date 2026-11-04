# Laporan Hasil Uji - Chapter 9 (Membangun Antarmuka dengan Vue)

Nama Penguji: Hanifa Nurul Khaira
NIM: 23367027

## 1. Ringkasan Eksekusi & Pengujian
Pengujian dilakukan secara lokal pada antarmuka frontend Vue 3 dan Vite menggunakan data tiruan (fixture) reaktif di memori (`src/data/penawaran-latihan.js`). Seluruh komponen modular diuji berdasarkan skenario interaksi pengguna, validasi formulir, dan kompilasi produksi.

## 2. Aliran Props & Emits Antar Komponen
* `App.vue` (Komponen Utama): Mengelola state reaktif utama seperti daftar penawaran, kata kunci pencarian, kategori terpilih, mode tampilan (Pembeli vs Penyedia), serta penambahan data penawaran baru.
* `FilterPenawaran.vue`: Menerima *props* kategori dan nilai pencarian, serta menggunakan peristiwa (emits) `update:pencarian` dan `update:kategori` untuk mengirimkan perubahan input kembali ke `App.vue`.
* `KartuPenawaran.vue`: Menerima *props* data item penawaran individual (`penawaran`) untuk merender informasi produk, harga normal, harga diskon, stok, dan sisa waktu kedaluwarsa.
* `FormPenawaran.vue`: Menerima data masukan form dari penyedia dan memancarkan peristiwa (emits) `submit` untuk menambahkan data penawaran baru ke memori setelah lolos validasi.
* `Ikon.vue`: Berfungsi sebagai komponen pembantu modular untuk merender ikon SVG secara dinamis berdasarkan kategori atau status penawaran.

## 3. Perhitungan Ringkasan Pembeli & Penyedia
* Ringkasan Pembeli: Menghitung jumlah total penawaran aktif yang tersedia untuk diselamatkan berdasarkan waktu saat ini (real-time time checking), serta mengelompokkan kategori secara otomatis menggunakan `computed properties`.
* Ringkasan Penyedia (Lapau Uni Rina): Menghitung total penawaran milik penyedia aktif, membedakan status penawaran (aktif, draft, habis, atau kedaluwarsa), serta menampilkan rekapitulasi stok yang dikelola oleh penyedia bersangkutan.

## 4. Hasil Skenario Pengujian 
1. Katalog & Filter: Berhasil menampilkan daftar produk makanan berlebih dengan lancar. Pencarian nama dan filter kategori bekerja secara reaktif.
2. Empty State: Kondisi kosong (empty state) muncul dengan semestinya saat kata kunci pencarian atau kategori yang dipilih tidak ditemukan pada data tiruan.
3. Form Invalid: Formulir penawaran baru berhasil mendeteksi kesalahan dan menampilkan peringatan validasi ketika input dikosongkan atau diisi dengan data yang tidak valid.
4. Form Valid: Formulir berhasil melakukan submit dan memperbarui daftar katalog secara langsung di antarmuka ketika seluruh field diisi dengan benar.
5. Tampilan Mobile: Tata letak (layout) responsif beradaptasi dengan baik saat diuji menggunakan simulasi ukuran layar perangkat seluler.

## 5. Lampiran Dokumentasi Hasil Uji & Terminal

* Tampilan Katalog: Berhasil menampilkan antarmuka utama katalog SisaBaik dengan komponen banner, statistik data, kotak pencarian, filter kategori, serta kartu penawaran produk makanan berlebih secara reaktif.
* Tampilan Pencarian Kosong (Empty State): Berhasil menampilkan pesan informasi kosong ketika kata kunci pencarian atau kategori yang dipilih tidak ditemukan dalam data tiruan (fixture).
* Tampilan Validasi Formulir Gagal (Form Invalid): Berhasil memicu sistem validasi saat input data penyedia dikosongkan atau diisi dengan format yang tidak sesuai, sehingga pesan peringatan error muncul dengan benar.
* Tampilan Formulir Berhasil (Form Valid): Berhasil memproses penyimpanan data penawaran baru melalui formulir yang valid, di mana data langsung masuk dan memperbarui daftar katalog di halaman utama.
* Tampilan Responsif Seluler (Mobile View): Berhasil menguji tata letak responsif menggunakan simulasi perangkat seluler melalui Developer Tools, di mana seluruh elemen antarmuka menyesuaikan ukuran layar dengan baik.
* Hasil Proses Terminal Build: Berhasil menjalankan perintah kompilasi produksi `npm run build` menggunakan Vite tanpa kendala atau error, menghasilkan direktori folder `dist/` yang siap didistribusikan.