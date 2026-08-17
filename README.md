# Analisis Penjualan Spare Part Vespa — Studi Kasus Data Analyst

Studi kasus analisis data penjualan riil sebuah toko spare part Vespa di Bandung, menggunakan Microsoft Excel (PivotTable, formula, dan dashboard visual). Nama toko disamarkan atas permintaan pemilik data.

## Konteks & Masalah Bisnis

Toko X, penjual spare part Vespa di Bandung, ingin memahami pola penjualannya untuk menemukan strategi yang bisa menaikkan sales secara keseluruhan. Studi kasus ini mengeksplorasi pertanyaan tersebut menggunakan data transaksi 3 bulan terakhir.

## Data & Keterbatasan

Dataset berisi 4 kolom: `tanggal`, `nama barang`, `qty`, `total harga` — 1 baris merepresentasikan 1 item terjual. Cakupan 3 bulan, total omset sekitar Rp13 juta.

**Proses pengumpulan data:** pencatatan asli dilakukan secara manual (tulis tangan) oleh toko, kemudian difoto dan dikonversi menjadi tabel digital menggunakan bantuan AI (OCR/ekstraksi teks dari gambar). Hasil konversi diverifikasi ulang baris per baris secara manual untuk memastikan kesesuaian dengan catatan asli, dilanjutkan dengan proses data cleaning sebelum dianalisis.

Keterbatasan yang secara sadar membentuk ruang lingkup analisis:

- **Tidak ada data cost/margin** → analisis dibatasi pada revenue, bukan profitabilitas.
- **Tidak ada customer ID atau transaction ID** → analisis retensi pelanggan dan market basket analysis tidak bisa dilakukan.
- **Hanya 3 bulan & omset kecil (skala transaksi harian rendah)** → analisis pola waktu granular (mis. deteksi stockout per minggu per barang) diputuskan **tidak dilanjutkan** karena pada skala ini gap mingguan adalah noise normal, bukan sinyal yang bisa diandalkan.
- **Toko tutup hari Minggu** (dikonfirmasi) → pola mingguan hanya mencakup Senin–Sabtu.

## Metodologi

1. Pembersihan data & pembuatan kolom bantu (harga satuan, hari, bulan).
2. **Pareto/distribusi revenue** — identifikasi seberapa terkonsentrasi (atau tersebar) kontribusi revenue antar SKU.
3. **Volume vs revenue mismatch** — memisahkan barang "laris tapi murah" dari "jarang tapi mahal".
4. **Pola waktu (indikatif)** — revenue rata-rata per hari, dengan catatan eksplisit soal keterbatasan sample.
5. Seluruh proses dilakukan dengan PivotTable Excel, dirangkum dalam satu dashboard visual.

## Temuan Utama

![Dashboard Ringkasan](assets/dashboard-screenshot.png)

- **Revenue tersebar merata, bukan terkonsentrasi (pola *long tail*).** Dari 221 SKU aktif, top 5 SKU hanya menyumbang **15%** dari total revenue Rp13.801.500 — jauh dari pola Pareto 80/20 yang umum diasumsikan di ritel. Bahkan top 8 barang gabungan (Oli Mesin hingga Laher NTN) hanya berkontribusi sekitar 20% dari total. Ini mengindikasikan pelanggan datang untuk kebutuhan spesifik (part yang aus/rusak pada motor masing-masing), bukan membeli dari segelintir barang favorit — rata-rata tiap SKU hanya terjual **531 ÷ 221 ≈ 2,4 unit** dalam 3 bulan.
- **Pengecualian: Oli Mesin.** Dengan revenue Rp894.000, lebih dari 2x lipat barang kedua (Kampas rem, Rp361.000). Ini konsisten dengan sifatnya sebagai barang consumable rutin (dibeli berkala, bukan hanya saat rusak) — berbeda karakter dari mayoritas SKU lain, dan menjadi satu-satunya barang dengan pola menonjol jelas dari data.
- **Pola volume-vs-revenue** menunjukkan sebaran barang murah-bervolume lebih tinggi vs barang bernilai lebih tinggi-bervolume rendah — namun dengan sifat long tail di atas, tidak ada kelompok "fast-mover" yang benar-benar dominan untuk dijadikan anchor strategi tunggal.
- **Pola harian** (Senin–Sabtu, toko tutup Minggu) — indikatif saja, mengingat volume transaksi harian yang kecil membuat rata-rata mudah bergeser oleh satu-dua transaksi besar.

## Rekomendasi Bisnis

Karena sebaran revenue bersifat long tail (bukan didominasi segelintir barang), strategi diarahkan ke menjaga keberagaman stok dan memperbesar nilai per kunjungan, bukan berfokus sempit pada beberapa "barang terlaris".

- **Jaga ketersediaan stok secara luas (breadth), bukan hanya beberapa item.** Karena tidak ada SKU yang benar-benar dominan (top 5 hanya 15% revenue), risiko kehilangan sales lebih besar berasal dari kekosongan salah satu dari ratusan part yang jarang dibeli tapi dibutuhkan spesifik, dibanding kehabisan satu "best-seller".
- **Perlakukan Oli Mesin secara khusus.** Sebagai satu-satunya barang dengan pola consumable rutin dan revenue menonjol, ini kandidat realistis untuk dijaga stoknya secara ketat dan dipertimbangkan sebagai starting point program pelanggan rutin (mis. pengingat servis berkala).
- **Promo di periode sepi** (berdasarkan pola harian Senin–Sabtu, meski masih indikatif) berpotensi menaikkan volume total, dibanding promo di periode yang memang sudah ramai.
- **Strategi jangka panjang berbasis retensi pelanggan** memerlukan pencatatan data pembeli (nama/nomor HP) yang saat ini belum tersedia — ini rekomendasi struktural untuk pengumpulan data ke depan, bukan sesuatu yang bisa dijawab dari dataset saat ini.

## Rekomendasi Operasional: Digitalisasi Pendataan

Proses pengumpulan data saat ini (tulis tangan → foto → konversi AI → verifikasi manual) rawan menimbulkan kesalahan pencatatan dan memakan waktu ekstra untuk verifikasi setiap kali data ingin dianalisis. Rekomendasi: beralih ke pencatatan digital langsung di titik penjualan, misalnya:

- **Aplikasi kasir sederhana (POS) berbasis mobile/tablet** — banyak opsi gratis/murah yang sudah punya fitur pencatatan barang, qty, dan harga otomatis per transaksi.
- **Minimal, spreadsheet digital langsung** (Google Sheets diisi manual saat transaksi) — lebih sederhana dari POS, tapi tetap menghilangkan langkah foto dan konversi AI.
- **Manfaat tambahan** dari pendataan digital: membuka peluang mencatat identitas pembeli (nomor HP) secara alami di titik transaksi, yang saat ini menjadi keterbatasan utama data — sehingga analisis retensi pelanggan ke depan menjadi mungkin dilakukan.

## Refleksi

Bagian tersulit dari studi kasus ini bukan teknik analisisnya, melainkan menentukan teknik mana yang **tidak** layak dipakai pada skala data sekecil ini (misalnya analisis stockout mingguan yang awalnya direncanakan, lalu dibatalkan setelah mempertimbangkan ukuran sample). Keputusan itu sendiri adalah bagian dari analisis.

## Struktur Repo

```
├── README.md
├── assets/
│   └── dashboard-screenshot.png
└── vespa-sales-analysis.xlsx
```

## Tools

Microsoft Excel — PivotTable, formula (SUMIFS, SUMPRODUCT), chart (bar, scatter), dashboard layout manual.
