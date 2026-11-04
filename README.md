<i>Developed by Hanifa Nurul Khaira (23367027)</i>

# SisaBaik - Frontend Vue (Chapter 9)

Proyek antarmuka pengguna (frontend) untuk website SisaBaik yang dibangun menggunakan Vue 3, Vite, dan arsitektur berbasis komponen modular. Modul ini fokus pada pembangunan antarmuka interaktif, pengelolaan reaktifitas (computed, state), props, emits, serta penggunaan data tiruan (fixture) di memori.

## Capaian Modul 9 (Membangun Antarmuka dengan Vue)
- Menginisialisasi struktur aplikasi frontend berbasis Vue 3 dan Vite di dalam direktori terpisah (`/frontend`) dengan manajemen ketergantungan mandiri.
- Menerapkan arsitektur berbasis komponen modular (`FilterPenawaran.vue`, `FormPenawaran.vue`, `KartuPenawaran.vue`, `Ikon.vue`, dan `App.vue`) dengan implementasi props, emits, dan computed properties.
- Menggunakan data tiruan berbasis memori (`src/data/penawaran-latihan.js`) untuk pengujian reaktif antarmuka secara mandiri.
- Menggunakan file aset SVG kependekan jenama dan ikon makanan.
- Melakukan proses kompilasi produksi (build) menggunakan Vite serta memverifikasi hasil keluarannya melalui server preview lokal.

## Struktur Direktori Proyek
- `/frontend/src/components`: Berisi komponen modular Vue (`App.vue`, `FilterPenawaran.vue`, `FormPenawaran.vue`, `KartuPenawaran.vue`, `Ikon.vue`).
- `/frontend/src/data`: Berisi file fixture data latihan (`penawaran-latihan.js`).
- `/frontend/src/lib`: Berisi pustaka pendukung penulisan dan visualisasi data.
- `/frontend/src/assets`: Berisi aset SVG ikon dan logo jenama.
- `/frontend/dist`: Hasil keluaran kompilasi (build) produksi.

## Menjalankan Frontend Vue

Untuk menginstal dependensi, menjalankan server pengembangan (development), melakukan build produksi, serta menguji pratinjau lokal, jalankan perintah berikut secara berurutan di terminal (pastikan posisi terminal berada di dalam folder `frontend`):

```bash
cd frontend
npm install
npm run dev
npm run build
npm run preview